task :build do 
  system "make clean"
  system "make singlehtml"
end

task :deploy do
  Rake::Task['build'].invoke

  # Copy the build documentation to the server.
  puts "Transfering sphinx build to vandpibe.bjrnskov.dk"
  system "rsync -vcqPr --delete _build/singlehtml/* piglet:www/vandpibe/web"
end

task :default => :build
