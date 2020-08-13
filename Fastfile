

require "spaceship"
default_platform(:ios)

groups = nil,
group = nil

platform :ios do
  
  slack_hook = ""
  
    #-----------------------------------Phase II, delete me when begining phase II
    desc "This lane will create a new app on Apple Dev & iTunes Connect"
    lane :new_app do
    produce(
    username: "",
    app_identifier: "",
    app_name: "Johnny's test produce",
    team_name: "",
    itc_team_name: ""
    )
  end
  
  desc "Automate management of development certificates - This will check if any of the available signing certificates is installed on your local machine.
        sigh can create, renew, download and repair provisioning profiles (with one command). 
        It supports App Store, Ad Hoc, Development and Enterprise profiles and supports nice features, like auto-adding all test devices."
  lane :get_dev_certs do
    cert(development: true)
    sigh(development: true)
  end

  desc "Re-obtain match code-signing credentials after switching to a Beginning or Ending project folder"
  lane :bootstrap_code_signing do
    sync_device_info
    match(type: "development")
    match(type: "adhoc")
    match(type: "appstore")
  end
  
  desc "Update iOS UDID's on the Developer Portal"
  lane :sync_device_info do
    register_devices(
      devices: {
        "device name" => "UUID",
        "another device" => "UUID"
      }
    )
    end
  
  desc "Sync team Code-Signing assets"
  lane :sync_signing_assets do |options|
    sync_device_info
    selectedType = options[:type]
    match(type: selectedType)
  end
  #-----------------------------------Phase II, delete me when begining phase II  -----------------------------------#
  






  

#----------------------------------- Phase I, delete me when begining phase II -----------------------------------#

  desc "Compiles app for App Store submission"
  lane :build do #build is stored in build_Appstore directory
    
    ####ensure_git_status_clean - Not Using for now
    # ensure_git_branch(branch: "staging")
    # git_pull
    # ####sh <call Daryl's existing build script>####
    
    #------------------additional features currently not using
    # scan - Provides an easy way to to run tests of iOS Apps
    # lint
    # document
    #------------------additional features currently not using

    #sync_signing_assets(type: "appstore") --Referencing lane sync_signing_assets-- Phase II
    increment_build_number
    gym(
    output_directory: "build_Appstore",
    export_method: "app-store"
    )
    # commit_version_bump(
    #  force: true,
    #  message: "Version bumped by fastlane"
    #  )
    # add_git_tag(
    # grouping: "fastlane",
    # build_number: lane_context[SharedValues::BUILD_NUMBER]
    # )
    # push_to_git_remote
    
    # v = lane_context[SharedValues::VERSION_NUMBER]
    # b = lane_context[SharedValues::BUILD_NUMBER]
    # d = sh "date -u"
    # slack(
    
    # payload: {"FASTLANE BETA BUILD — #{v}(#{b})" => d},
    # slack_url: slack_hook
    
    #)
  end
  
  desc "Compiles app for Ad Hoc submission. Distribute app to testers on registered devices only. Apps expire in a few days and will stop working."
  lane :build_adhoc do
    
    ensure_git_status_clean
    ensure_git_branch(branch: "staging")
    git_pull
    
    #------------------additional features currently not using
    # scan 
    # lint
    # document
    #------------------additional features currently not using
    
    #sync_signing_assets(type: "adhoc") --Referencing lane sync_signing_assets-- Phase II
    increment_build_number
    gym(
    output_directory: "build_AdHoc",
    export_method: "ad-hoc"
    )
    
    commit_version_bump(
    force: true,
    message: "Version bumped by fastlane"
    )
    add_git_tag(
    grouping: "fastlane",
    build_number: lane_context[SharedValues::BUILD_NUMBER]
    )
    push_to_git_remote
    
  end
  
####################################################### Starting first implementation #######################################################

   desc "Calls on build_appstore, which then will increment build number and compiles app along with distributing to TestFlight. App expires after 90 days."
   lane :demo do
    build # --Referencing lane build_appstore to compile app
    pilot(
      beta_app_review_info: {
        contact_email: "",
        contact_first_name: "",
        contact_last_name: "",
        contact_phone: "",
        demo_account_required: true,
        demo_account_name: "",
        demo_account_password: ""#,
        #notes: "Test video calls and document workflows."
      },

      localized_app_info: {
        "default": {
          feedback_email: "",
          marketing_url: "",
          privacy_policy_url: "",
          description: "",
        },
      
      localized_build_info: {
        "default": {
          whats_new: "Default changelog",
        },
        # "en-US": {
        #   whats_new: "en changelog",
        # }
      },
    uses_non_exempt_encryption: true
      }
    )
  end
  
  lane :prod do
    precheck
    build_appstore
    snapshot
    frameit(gold: true)
    deliver(
    ipa: "./build_AppStore/ChewChewTrain.ipa",
    run_precheck_before_submit: false,
    force: true,
    team_name: "STRING The name of your Developer Portal team if you're in multiple teams"
    )
  end
  
  lane :lint do
    swiftlint(
    mode: :autocorrect,
    config_file: ".swiftlint.yml",
    output_file: "swiftlintOutput.txt",
    ignore_exit_status: false
    )
  end
  
  # lane :document do
  #   jazzy config: "./fastlane/jazzy.yaml"
  # end

  # lane :ping_slack do
    
    # slack(
    # message: "Test Message",
    # slack_url: slack_hook,
    # default_payloads: [],
    # payload: {"PAY" => "LOAD"}
    # )
  # lane :fill_test_info do #should not be needed, but setting feedback_email does not currently work with pilot...
  #   fastlane_require "spaceship"
  #     Spaceship.login
  #     Spaceship::Tunes.login("$APPLE_ID_EMAIL$")
  #     app = Spaceship::Tunes::Application.find("com.popinmobilebanking.cct.demo")
  #     app_info = Spaceship::TestFlight::AppTestInfo.find(app_id: app.apple_id)
  #     app_info.test_info.feedback_email = "jmartinez@popio.com";
  #     app_info.beta_review_info.contact_email = "jmartinez@popio.com";
  #     app_info.beta_review_info.contact_first_name = "Johnny";
  #     app_info.beta_review_info.contact_last_name = "Martinez";
  #     app_info.beta_review_info.contact_phone = "8015734861";
  #     app_info.save_for_app!(app_id: app.apple_id)
  #     UI.message "Beta app feedback_email set"
  # end
  
  lane :new_app do
      fastlane_require "spaceship"
      #Spaceship.login
      Spaceship::Tunes.login("admin@financialtown.com")
      app = Spaceship::Tunes::Application.find("")
          groups = Spaceship::TestFlight::Group.filter_groups(app_id: app.apple_id) { |group| group.name == "External Testers" }
      if(not groups.kind_of?(Array) or groups.length <= 0)
        UI.message "Tester group not found. Creating one..."
          group = Spaceship::TestFlight::Group.create!(app_id: com.popinvideobanking.demo, group_name: "External Testers")
      else
        UI.message "Tester group already exists! Skipping."
      end
    end
  
  lane :johnny_test do
    increment_build_number
    gym(
      output_directory: "build_Appstore",
      export_method: "app-store"
      )
    pilot(
      username: "admin@financialtown.com",
      groups: ['Extermal Testers'],
      app_identifier: "com.popinvideobanking.cct.demo",
      ipa: "build_Appstore/ChewChewTrain.ipa"
    )
  end

#   lane :create_group do
#     fastlane require "spaceship" 
#     group = Spaceship::TestFlight::Group.create!(app_id: 'com.popinvideobanking.cct.demo', group_name: 'External Testers')
#   end
end
