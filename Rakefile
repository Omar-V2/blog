BASENAME = File.basename(Dir.getwd)

USER = "lenni"
HOST = "leonard.io"
PATH = "www/#{BASENAME}"

task :default => ["server"]

desc "Deploys the content of this folder minus the .git directory"
task :deploy do
  sh "jekyll build"
  puts "rsyncing... "
  sh "rsync -rC _site/ #{USER}@#{HOST}:#{PATH}"
  html = `ls -l _posts/|grep html|wc -l`.strip
  md = `ls -l _posts/|grep md|wc -l`.strip
  puts ""
  puts "HTML:     #{html}"
  puts "Markdown: #{md}"
end

task :server do
  sh "bundle exec jekyll serve --watch --baseurl=/blog --incremental"
end

task :list do
  sh "ls -l _posts/|grep html"
end

task :clean do
  rm_rf "_site"
end
