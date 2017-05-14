desc "Build and deploy the site"
task :deploy do
  system "jekyll build && rsync -avze 'ssh -p 22' --delete _site/ perchr@thalos:/var/www/eastblue.org/htdocs/blag"
end