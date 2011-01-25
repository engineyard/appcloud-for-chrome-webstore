require 'rake/packagetask'

Rake::PackageTask.new("chrome-appstore-upload", "1.0.0") do |p|
  p.need_zip = true
  p.package_files.include("icon*.png")
  p.package_files.include("manifest.json")
end

desc "Release to Chrome Web Store"
task :release => :package do
  puts "Ok, so this is manual at the moment. Your browser is opening now..."
  `open "https://chrome.google.com/webstore/developer/update?hl=en-US"`
end