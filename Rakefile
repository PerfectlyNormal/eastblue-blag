task default: :serve

desc "Build and deploy the site"
task :deploy do
  system "jekyll build && rsync -avze 'ssh -p 22' --delete _site/ perchr@eastblue.org:/var/www/eastblue.org/htdocs/blag"
end

desc "Start server"
task :serve do
  system "jekyll serve"
end