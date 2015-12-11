# Configurable
def name
  "Moya-Extension"
end

def bundle_id
  "com.moya.extension"
end

# Project Creation
def target_identifier(platform)
  case platform
  when :ios
    "iOS"
  when :osx
    "Mac"
  when :tvos
    "tvOS"
  when :watchos
    "watchOS"
  else
    abort "platform not supported!"
  end
end

def target_settings(target, platform, version)

  target_suffix = target_identifier platform

  target.name = "#{name}-#{target_suffix}"

  target.language = :swift
  target.type = :framework
  target.platform = platform
  target.deployment_target = version

  target.include_files = ["Source/*.swift", "Supporting Files/*.h"]
  target.all_configurations.settings["INFOPLIST_FILE"] = "Supporting Files/Info.plist"
  target.all_configurations.settings["PRODUCT_BUNDLE_IDENTIFIER"] = bundle_id
  target.all_configurations.settings["RAW_PRODUCT_NAME"] = name
  target.all_configurations.settings["PRODUCT_NAME"] = "${RAW_PRODUCT_NAME:identifier}"
  target.all_configurations.settings["ENABLE_BITCODE"] = platform == :osx ? "NO" : "YES"
  target.all_configurations.settings["LD_RUNPATH_SEARCH_PATHS"] = "$(inherited) @executable_path/Frameworks @loader_path/Frameworks"
  
end

def test_target_settings(project, host_target)

  unit_test_target = project.target do |target|

    target.name = "#{host_target.name}Tests"

    target.language = host_target.language
    target.type = :unit_test_bundle
    target.platform = host_target.platform
    target.deployment_target = host_target.deployment_target

    target.include_files = ["Tests/*.swift"]

    target.all_configurations.settings["INFOPLIST_FILE"] = "Supporting Files/Info.plist"
    target.all_configurations.settings["LD_RUNPATH_SEARCH_PATHS"] = "$(inherited) @executable_path/Frameworks @loader_path/Frameworks"

  end

end

Project.new name do |project|

  project.target do |target|
    target_settings target, :ios, 8.0
    test_target_settings project, target
  end

  project.target do |target|
    target_settings target, :osx, 10.9
    test_target_settings project, target
  end

  # project.target do |target|
  #   target_settings target, :tvos, 9.0
  #   test_target_settings project, target
  # end
  #
  # project.target do |target|
  #   target_settings target, :watchos, 2.0
  #   test_target_settings project, target
  # end
  #


end
