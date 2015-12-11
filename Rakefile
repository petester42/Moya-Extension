def supported_test_targets
  return [:ios, :osx, :tvos]
end

def supported_build_targets
  return [:ios, :osx, :tvos, :watchos]
end

def scheme_name
  return "Moya-Extension"
end

def project_name
  return "Moya-Extension.xcodeproj"
end

def settings_for_platform(platform)
  case platform
  when :ios
    return {
      sdk: 'iphonesimulator',
      scheme: "#{scheme_name}-iOS",
      device_host: "name='iPhone 6s'"
    }
  when :osx
    return {
      sdk: 'macosx',
      scheme: "#{scheme_name}-Mac",
      device_host: "arch='x86_64'"
    }
  when :tvos
    return {
      sdk: 'appletvsimulator',
      scheme: "#{scheme_name}-tvOS",
      device_host: "name='Apple TV 1080p'"
    }
  when :watchos
    return {
      sdk: 'watchsimulator',
      scheme: "#{scheme_name}-watchOS",
      device_host: "name='Apple Watch - 42mm'"
    }
  else
    abort "You have to specify a supported platform: :ios, :osx, :tvos"
  end
end

def xcodebuild(platform, configuration, tasks, xcprety_args)
  project = project_name
  configuration = configuration
  settings = settings_for_platform platform
  sdk = settings[:sdk]
  scheme = settings[:scheme]
  destination = settings[:device_host]

  sh "set -o pipefail && xcodebuild -project '#{project}' -scheme '#{scheme}' -configuration '#{configuration}' -sdk #{sdk} -destination #{destination} #{tasks} | xcpretty -c #{xcprety_args}"
end

desc 'Setup project'
task :setup do
  sh 'bundle exec xcake'
end

desc 'Build, then run tests.'
task :test do
  supported_test_targets.map { |platform| xcodebuild platform, 'Debug', 'clean build test', '--test' }
  sh 'killall Simulator'
end

desc 'Build the frameworks'
task :build do
  supported_build_targets.map { |platform| xcodebuild platform, 'Debug', 'clean build', '' }
  supported_build_targets.map { |platform| xcodebuild platform, 'Release', 'clean build', '' }
end
