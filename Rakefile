require "bundler/gem_tasks"

GEMS = %w(fastlane danger-device_grid)

SECONDS_PER_DAY = 60 * 60 * 24

task(:rubygems_admins) do
  names = ["KrauseFx", "ohayon", "mpirri", "taquitos"]
  (GEMS + ["krausefx-shenzhen", "commander-fastlane"]).each do |gem_name|
    names.each do |name|
      puts(`gem owner #{gem_name} -a #{name}`)
    end
  end
end

task(:test_all) do
  formatter = "--format progress"
  formatter += " -r rspec_junit_formatter --format RspecJunitFormatter -o #{ENV['CIRCLE_TEST_REPORTS']}/rspec/fastlane-junit-results.xml" if ENV["CIRCLE_TEST_REPORTS"]
  sh("rspec --pattern ./**/*_spec.rb #{formatter}")
end

# Overwrite the default rake task
# since we use fastlane to deploy fastlane
task(:push) do
  sh("bundle exec fastlane release")
end

task(:generate_team_table) do
  require 'json'
  content = ["<ul id='team-table' style='display:flex; flex-wrap:wrap; flex-direction:row; list-style:none;' max-width='100%'>"]

  contributors = JSON.parse(File.read("team.json"))
  contributors.keys.shuffle.each do |github_user|
    user_content = contributors[github_user]

    content << "<li style='padding: 6px; border: 1px solid #dfe2e5;'>"
    content << "<img src='https://github.com/#{github_user}.png?size=200' width=140>"
    content << "<h4 align='center'><a href='https://twitter.com/#{user_content['twitter']}'>#{user_content['name']}</a></h4>"
    # content << "<p align='center'>#{user_content['slogan']}</p>" if user_content['slogan'].to_s.length > 0

    content << "</li>"
  end
  content << "</ul>"

  readme = File.read("README.md")
  readme.gsub!(%r{\<ul id='team-table'.*\<\/ul\>}m, content.join("\n"))
  File.write("README.md", readme)
  puts("All done")
end

#####################################################
# @!group Helper Methods
#####################################################

def box(str)
  l = str.length + 4
  puts('')
  puts('=' * l)
  puts('| ' + str + ' |')
  puts('=' * l)
end

task(default: :test_all)
