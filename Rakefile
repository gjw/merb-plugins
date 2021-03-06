require "rake"
require "fileutils"
require "merb-core/tasks/merb_rake_helper"

gems = %w[merb_activerecord merb_helpers merb_sequel merb_param_protection merb_test_unit merb_stories merb_screw_unit]

# Implement standard Rake::GemPackageTask tasks - see merb.thor
task :clobber_package do; FileUtils.rm_rf('pkg'); end
task :package do; end

desc "Install all gems"
task :install do
  Merb::RakeHelper.install('merb-plugins')
end

desc "Uninstall all gems"
task :uninstall => :uninstall_gems

desc "Build the merb-more gems"
task :build_gems do
  gems.each do |dir|
    Dir.chdir(dir) { sh "#{Gem.ruby} -S rake package" }
  end
end

desc "Install the merb-more sub-gems"
task :install_gems do
  gems.each do |dir|
    Dir.chdir(dir) { sh "#{Gem.ruby} -S rake install" }
  end
end

desc "Uninstall the merb-more sub-gems"
task :uninstall_gems do
  gems.each do |dir|
    Dir.chdir(dir) { sh "#{Gem.ruby} -S rake uninstall" }
  end
end

desc "Clobber the merb-more sub-gems"
task :clobber_gems do
  gems.each do |dir|
    Dir.chdir(dir) { sh "#{Gem.ruby} -S rake clobber" }
  end
end

desc "Bundle up all the merb-plugins gems"
task :bundle do
  mkdir_p "bundle"
  gems.each do |gem|
    File.open("#{gem}/Rakefile") do |rakefile|
      rakefile.read.detect {|l| l =~ /^GEM_VERSION\s*=\s*"(.*)"$/ }
      Dir.chdir(gem){ sh "rake package" }
      sh %{cp #{gem}/pkg/#{gem}-#{$1}.gem bundle/}
    end
  end
end

desc "Release gems in merb-plugins"
task :release do
  gems.each do |dir|
    Dir.chdir(dir){ sh "#{Gem.ruby} -S rake release" }
  end
end
