require 'byebug'
require 'zip'

namespace :android do
  desc "Generate icon files"
  task :generate_icon  do
    ARGV.each { |a| task a.to_sym do ; end }

    sizes = [ { name: 'mipmap-hdpi', size: '72x72' },
              { name: 'mipmap-mdpi', size: '48x48' }, 
              { name: 'mipmap-xhdpi', size: '96x96' }, 
              { name: 'mipmap-xxhdpi', size: '144x144' }, 
              { name: 'mipmap-xxxhdpi', size: '192x192' }]

    sizes.each do |size|
      FileUtils::mkdir_p  "#{ARGV[1]}/app/src/production/res/#{size[:name]}"
      `convert #{ARGV[1]}/branding/app_icon.png -resize #{size[:size]} #{ARGV[1]}/app/src/production/res/#{size[:name]}/ic_launcher.png`
    end
  end

  desc "Generate Branding Images"
  task :generate_branding  do
    ARGV.each { |a| task a.to_sym do ; end }
    source_folder = ARGV[1]

    FileUtils.cp "#{source_folder}/branding/about_android_phone_portrait.jpg",     "#{source_folder}/assets/interface-images/about-phone-portrait.jpg"
    FileUtils.cp "#{source_folder}/branding/about_android_tablet_landscape.jpg",   "#{source_folder}/assets/interface-images/about-tablet-landscape.jpg"
    FileUtils.cp "#{source_folder}/branding/about_android_tablet_portrait.jpg",    "#{source_folder}/assets/interface-images/about-tablet-portrait.jpg"

    FileUtils.cp "#{source_folder}/branding/introduction_android_phone_portrait.jpg",          "#{source_folder}/app/src/production/res/drawable/introduction.jpg"
    FileUtils.cp "#{source_folder}/branding/introduction_android_tablet_landscape.jpg",        "#{source_folder}/app/src/production/res/drawable-sw600dp-land/introduction.jpg"
    FileUtils.cp "#{source_folder}/branding/introduction_android_tablet_portrait.jpg",         "#{source_folder}/app/src/production/res/drawable-sw600dp-port/introduction.jpg"
    FileUtils.cp "#{source_folder}/branding/introduction_bottom_android_phone_portrait.png",   "#{source_folder}/app/src/production/res/drawable/introduction_bottom.png"
    FileUtils.cp "#{source_folder}/branding/introduction_bottom_android_tablet_landscape.png", "#{source_folder}/app/src/production/res/drawable-sw600dp-land/introduction_bottom.png"
    FileUtils.cp "#{source_folder}/branding/introduction_bottom_android_tablet_portrait.png",  "#{source_folder}/app/src/production/res/drawable-sw600dp-port/introduction_bottom.png"
    FileUtils.cp "#{source_folder}/branding/introduction_top_android_phone_portrait.png",      "#{source_folder}/app/src/production/res/drawable/introduction_top.png"
    FileUtils.cp "#{source_folder}/branding/introduction_top_android_tablet_landscape.png",    "#{source_folder}/app/src/production/res/drawable-sw600dp-land/introduction_top.png"
    FileUtils.cp "#{source_folder}/branding/introduction_top_android_tablet_portrait.png",     "#{source_folder}/app/src/production/res/drawable-sw600dp-port/introduction_top.png'"

    FileUtils.cp "#{source_folder}/branding/splash_android_phone_portrait.jpg",                "#{source_folder}/app/src/production/res/drawable/splash.jpg"
    FileUtils.cp "#{source_folder}/branding/splash_android_tablet_landscape.jpg",              "#{source_folder}/app/src/production/res/drawable-sw600dp-land/splash.jpg"
    FileUtils.cp "#{source_folder}/branding/splash_android_tablet_portrait.jpg",               "#{source_folder}/app/src/production/res/drawable-sw600dp-port/splash.jpg"
    FileUtils.cp "#{source_folder}/branding/splash_bottom_android_phone_portrait.png",         "#{source_folder}/app/src/production/res/drawable/splash_bottom.png"
    FileUtils.cp "#{source_folder}/branding/splash_bottom_android_tablet_landscape.png",       "#{source_folder}/app/src/production/res/drawable-sw600dp-land/splash_bottom.png"
    FileUtils.cp "#{source_folder}/branding/splash_bottom_android_tablet_portrait.png",        "#{source_folder}/app/src/production/res/drawable-sw600dp-port/splash_bottom.png"
    FileUtils.cp "#{source_folder}/branding/splash_top_android_phone_portrait.png",            "#{source_folder}/app/src/production/res/drawable/splash_top.png"
    FileUtils.cp "#{source_folder}/branding/splash_top_android_tablet_landscape.png",          "#{source_folder}/app/src/production/res/drawable-sw600dp-land/splash_top.png"
    FileUtils.cp "#{source_folder}/branding/splash_top_android_tablet_portrait.png",           "#{source_folder}/app/src/production/res/drawable-sw600dp-port/splash_top.png"

  end

  desc "Copy to destination"
  task :copy do
    ARGV.each { |a| task a.to_sym do ; end }
    source_folder      = ARGV[1]
    destination_folder = ARGV[2]

    # Remove old files
    FileUtils.rm_f "#{destination_folder}/app/flavors.gradle"
    FileUtils.rm_f "#{destination_folder}/app/google-services.json"
    FileUtils.rm_rf "#{destination_folder}/assets"
    FileUtils.rm_rf "#{destination_folder}/app/src/productionNoCrashReporting/res"
    FileUtils.rm_rf "#{destination_folder}/app/src/productionCrashReporting/res"

    # Remove Icon files
    Dir.glob(File.join("**", "Icon\r")).each {|f| FileUtils.rm(f)}

    #Copy to the app
    FileUtils.cp   "#{source_folder}/app/flavors.gradle",        "#{destination_folder}/app/flavors.gradle"
    FileUtils.cp   "#{source_folder}/app/google-services.json",  "#{destination_folder}/app/google-services.json"
    FileUtils.cp_r "#{source_folder}/app/src/production/res",    "#{destination_folder}/app/src/productionNoCrashReporting/res"
    FileUtils.cp_r "#{source_folder}/app/src/production/res",    "#{destination_folder}/app/src/productionCrashReporting/res"
  end

  desc "Zip assets folder"
  task :zip do
    ARGV.each { |a| task a.to_sym do ; end }
    source_folder = ARGV[1]

    app_id = ""
    version_code = ""

    contents = File.open("#{source_folder}/app/flavors.gradle").read
    contents.each_line do |line|
      line.strip!
      app_id = line.split[2].gsub('"','') if line.start_with?('appId')
      version_code = line.split[2].gsub('"','') if line.start_with?('versionCode')
    end
    zip_file_name = "main.#{version_code}.#{app_id}.obb"
    FileUtils.rm_rf zip_file_name
    `pushd #{source_folder}; zip -r #{File.dirname(__FILE__)}/#{zip_file_name} assets`

  end

  desc "Bootstrap Project. USAGE: rake android:bootstrap path/to/source/folder path/to/destination/folder"
  task :bootstrap  do
    ARGV.each { |a| task a.to_sym do ; end }
    source_folder      = ARGV[1]
    destination_folder = ARGV[2]
    if destination_folder.nil?
      puts "Must supply both a source and destination folder"
    elsif Dir.exists?(source_folder) == false || Dir.exists?(destination_folder) == false
      puts "source folder does not exist"      if Dir.exists?(source_folder) == false
      puts "destination folder does not exist" if Dir.exists?(destination_folder) == false
    else
      Rake::Task["android:generate_icon"].invoke(source_folder)
      Rake::Task["android:generate_branding"].invoke(source_folder)
      Rake::Task["android:copy"].invoke(source_folder, destination_folder)
      Rake::Task["android:zip"].invoke(source_folder)
    end
  end
end