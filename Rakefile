require 'bundler'
Bundler.require
Bundler::GemHelper.install_tasks

require 'opal/rspec/rake_task'
Opal::RSpec::RakeTask.new(:default) do |s|
    s.index_path = 'spec/html/index.html.erb'
end

desc "Build build/opal-pixi.js"
task :dist do
  require 'fileutils'
  FileUtils.mkdir_p 'build'

  src = Opal::Builder.build('opal-pixi')
  #min = uglify src
  #gzp = gzip min

  File.open('build/opal-pixi.js', 'w+') do |out|
    out << src
  end

  #puts "development: #{src.size}, minified: #{min.size}, gzipped: #{gzp.size}"
end

# Used for uglifying source to minify
def uglify(str)
  IO.popen('uglifyjs', 'r+') do |i|
    i.puts str
    i.close_write
    return i.read
  end
rescue Errno::ENOENT
  $stderr.puts '"uglifyjs" command not found (install with: "npm install -g uglify-js")'
  nil
end

# Gzip code to check file size
def gzip(str)
  IO.popen('gzip -f', 'r+') do |i|
    i.puts str
    i.close_write
    return i.read
  end
rescue Errno::ENOENT
  $stderr.puts '"gzip" command not found, it is required to produce the .gz version'
  nil
end
