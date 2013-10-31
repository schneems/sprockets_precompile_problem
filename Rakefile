# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require File.expand_path('../config/application', __FILE__)

SprocketsNoDigestUpdate::Application.load_tasks

def sort_files(files)
  stat_hash = Hash.new {|hash, key| hash[key] = File.stat(key)}
  files.map.sort_by {|f| stat_hash[f].mtime }
end


task :precompile_problem do
  if Dir.exist?("public/assets")
    puts "== Cleaning public/assets"
    FileUtils.remove_entry_secure("public/assets")
  end

  stylesheet = "app/assets/stylesheets/application.css.erb"
  puts "== Writing Stylesheet"
  File.open(stylesheet, 'w') {|f| f << "body {background-color:black};\n"}

  puts "== Running assets:precompile RAILS_ENV=production"
  puts `bundle exec rake assets:precompile RAILS_ENV=production`
  first_css = Dir.glob("public/assets/*.css").last
  first_js  = Dir.glob("public/assets/*.js").last

  puts "== Changing CSS File (should change link in JS file)"
  File.open(stylesheet, 'a') {|f| f << "html {background-color:red};\n"}

  puts "== Cleaning Assets"
  puts `bundle exec rake assets:clean`
  puts "== Running assets:precompile RAILS_ENV=production"
  puts `bundle exec rake assets:precompile RAILS_ENV=production`

  stylesheets = sort_files(Dir.glob("public/assets/*.css"))
  js          = sort_files(Dir.glob("public/assets/*.js"))

  second_css = stylesheets.last
  second_js  = js.last
  puts "== Contents of JS: (should be different)"
  puts ""
  puts "============= RESULTS ================="
  puts "First css:    '#{first_css}'"
  puts "Modified css: '#{second_css}'"
  puts "First JS:     '#{File.read(first_js)}'"
  puts "Modified JS:  '#{File.read(second_js)}'"
end
