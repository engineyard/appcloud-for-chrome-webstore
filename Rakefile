require 'rake/packagetask'
require File.expand_path("../lib/version.rb", __FILE__)

Rake::PackageTask.new("chrome-appstore-upload", "1.0.0") do |p|
  p.need_zip = true
  p.package_files.include("icon*.png")
  p.package_files.include("manifest.json")
end

def bump
  version_file = "module App\n  VERSION = '_VERSION_GOES_HERE_'\nend\n"

  new_version = if App::VERSION =~ /\.pre$/
                  App::VERSION.gsub(/\.pre$/, '')
                else
                  digits = App::VERSION.scan(/(\d+)/).map { |x| x.first.to_i }
                  digits[-1] += 1
                  digits.join('.') + ".pre"
                end

  puts "New version is #{new_version}"
  File.open('lib/version.rb', 'w') do |f|
    f.write version_file.gsub(/_VERSION_GOES_HERE_/, new_version)
  end
  new_version
end

def run_commands(*cmds)
  cmds.flatten.each do |c|
    system(c) or raise "Command "#{c}" failed to execute; aborting!"
  end
end

desc "Displays the current version"
task :version do
  puts "Current version: #{App.version}"
end

desc "Release to Chrome Web Store"
task :release => :package do
  new_version = bump

  run_commands(
    "git add lib/version.rb",
    "git commit -m 'Bump versions for release #{new_version}'")

  load 'lib/version.rb'
  bump

  run_commands(
    "git add lib/version.rb",
    "git commit -m 'Add .pre for next release'",
    "git tag v#{new_version} HEAD^")
  
  puts "Ok, so this is manual at the moment. Your browser is opening now..."
  `open "https://chrome.google.com/webstore/developer/edit/odbehbafobcdhgmfclophcpfndionpgi"`
end