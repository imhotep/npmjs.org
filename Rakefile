DIR = "www/attachments"
PORT = 9292

task :default => :server

task :watch do
  require 'listen'
  Listen.to(DIR, :filter => /\.less$/) do |modified, added, removed|
    Rake::Task[:less].execute
  end
end

task :less do
  system "lessc -x #{DIR}/layout.less > #{DIR}/layout.css"
  puts "compled less"
end

task :server do 
  require 'em/pure_ruby'
  require 'rack'
  require 'rack-rewrite'
  require 'thin'

  site = Rack::Builder.new do 
    use Rack::Rewrite do
      rewrite "/", "/index.html"
    end
    run Rack::Directory.new(DIR)
  end
  puts ">> Serving: #{DIR}/"
  Rack::Handler::Thin.run(site, :Port => PORT)
end
