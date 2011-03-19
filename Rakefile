module VIM
  Dirs = %w[ autoload bundle janus_bundle janus_bundle/janus_themes/colors ]
end

directory "tmp"
VIM::Dirs.each do |dir|
  directory(dir)
end

def vim_plugin_task(name, repo=nil)
  cwd = File.expand_path("../", __FILE__)
  dir = File.expand_path("tmp/#{name}")
  bundle_target = File.expand_path("janus_bundle/#{name}")
  subdirs = VIM::Dirs

  namespace(name) do
    if repo
      file dir => "tmp" do
        if repo =~ /git$/
          sh "git clone #{repo} #{dir}"

        elsif repo =~ /download_script/
          if filename = `curl --silent --head #{repo} | grep attachment`[/filename=(.+)/,1]
            filename.strip!
            sh "curl #{repo} > tmp/#{filename}"
          else
            raise ArgumentError, 'unable to determine script type'
          end

        elsif repo =~ /(tar|gz|vba|zip)$/
          filename = File.basename(repo)
          sh "curl #{repo} > tmp/#{filename}"

        else
          raise ArgumentError, 'unrecognized source url for plugin'
        end

        case filename
        when /zip$/
          sh "unzip -o tmp/#{filename} -d #{dir}"

        when /tar\.gz$/
          dirname  = File.basename(filename, '.tar.gz')

          sh "tar zxvf tmp/#{filename}"
          sh "mv #{dirname} #{dir}"

        when /vba(\.gz)?$/
          if filename =~ /gz$/
            sh "gunzip -f tmp/#{filename}"
            filename = File.basename(filename, '.gz')
          end

          # TODO: move this into the install task
          mkdir_p dir
          lines = File.readlines("tmp/#{filename}")
          current = lines.shift until current =~ /finish$/ # find finish line

          while current = lines.shift
            # first line is the filename (possibly followed by garbage)
            # some vimballs use win32 style path separators
            path = current[/^(.+?)(\t\[{3}\d)?$/, 1].gsub '\\', '/'

            # then the size of the payload in lines
            current = lines.shift
            num_lines = current[/^(\d+)$/, 1].to_i

            # the data itself
            data = lines.slice!(0, num_lines).join

            # install the data
            Dir.chdir dir do
              mkdir_p File.dirname(path)
              File.open(path, 'w'){ |f| f.write(data) }
            end
          end
        end
      end

      task :pull => dir do
        if repo =~ /git$/
          Dir.chdir dir do
            sh "git pull"
          end
        end
      end

      task :install => [:pull] + subdirs do
        sh "cp -rf #{dir} #{bundle_target}"
        rm_r "#{bundle_target}/.git" if repo =~ /git$/
        Dir.chdir bundle_target do
          yield if block_given?
        end
      end
    else
      task :install => subdirs do
        yield if block_given?
      end
    end
  end

  desc "Install #{name} plugin"
  task name do
    puts
    puts "*" * 40
    puts "*#{"Installing #{name}".center(38)}*"
    puts "*" * 40
    puts
    Rake::Task["#{name}:install"].invoke
  end
  task :default => name
end
vim_plugin_task "pathogen.vim" do
  file 'pathogen.vim' => 'autoload' do
    sh "curl https://github.com/tpope/vim-pathogen/raw/master/autoload/pathogen.vim > autoload/pathogen.vim"
  end
end

# vim_plugin_task "ack.vim",          "git://github.com/mileszs/ack.vim.git"
# vim_plugin_task "color-sampler",    "git://github.com/vim-scripts/Color-Sampler-Pack.git"
# vim_plugin_task "conque",           "http://conque.googlecode.com/files/conque_1.1.tar.gz"
# vim_plugin_task "fugitive",         "git://github.com/tpope/vim-fugitive.git"
# vim_plugin_task "git",              "git://github.com/tpope/vim-git.git"
vim_plugin_task "haml",             "git://github.com/tpope/vim-haml.git"
vim_plugin_task "indent_object",    "git://github.com/michaeljsmith/vim-indent-object.git"
vim_plugin_task "javascript",       "git://github.com/pangloss/vim-javascript.git"
vim_plugin_task "jslint",           "git://github.com/hallettj/jslint.vim.git"
vim_plugin_task "markdown_preview", "git://github.com/robgleeson/vim-markdown-preview.git"
vim_plugin_task "nerdtree",         "git://github.com/wycats/nerdtree.git"
vim_plugin_task "nerdcommenter",    "git://github.com/ddollar/nerdcommenter.git"
vim_plugin_task "surround",         "git://github.com/tpope/vim-surround.git"
vim_plugin_task "taglist",          "git://github.com/vim-scripts/taglist.vim.git"
# vim_plugin_task "vividchalk",       "git://github.com/tpope/vim-vividchalk.git"
vim_plugin_task "supertab",         "git://github.com/ervandew/supertab.git"
vim_plugin_task "cucumber",         "git://github.com/tpope/vim-cucumber.git"
# vim_plugin_task "textile",          "git://github.com/timcharper/textile.vim.git"
# vim_plugin_task "rails",            "git://github.com/tpope/vim-rails.git"
# vim_plugin_task "rspec",            "git://github.com/taq/vim-rspec.git"
vim_plugin_task "zoomwin",          "git://github.com/vim-scripts/ZoomWin.git"
vim_plugin_task "snipmate",         "git://github.com/msanders/snipmate.vim.git"
vim_plugin_task "markdown",         "git://github.com/tpope/vim-markdown.git"
# vim_plugin_task "align",            "git://github.com/tsaleh/vim-align.git"
# vim_plugin_task "unimpaired",       "git://github.com/tpope/vim-unimpaired.git"
# vim_plugin_task "searchfold",       "git://github.com/vim-scripts/searchfold.vim.git"
# vim_plugin_task "endwise",          "git://github.com/tpope/vim-endwise.git"
# vim_plugin_task "irblack",          "git://github.com/wgibbs/vim-irblack.git"
# vim_plugin_task "vim-coffee-script","git://github.com/kchmck/vim-coffee-script.git"
vim_plugin_task "syntastic",        "git://github.com/scrooloose/syntastic.git"
vim_plugin_task "gist-vim",         "git://github.com/mattn/gist-vim.git"

vim_plugin_task "IR_white" do
  sh "curl https://github.com/squil/vim_colors/raw/04d696a1d16a934c13bd578bc0e5dab5afb7e903/IR_White.vim > janus_bundle/janus_themes/colors/IR_White.vim"
end

vim_plugin_task "molokai" do
  sh "curl http://www.vim.org/scripts/download_script.php?src_id=9750 > janus_bundle/janus_themes/colors/molokai.vim"
end
vim_plugin_task "mustasche" do
  FileUtils.mkdir_p "janus_bundle/mustache/syntax"
  sh "curl http://github.com/defunkt/mustache/raw/master/contrib/mustache.vim > janus_bundle/mustache/syntax/mustache.vim"
end

desc "Update the documentation"
task :update_docs do
  puts "Updating VIM Documentation..."
  system "vim -e -s <<-EOF\n:helptags ~/.vim/doc\n:quit\nEOF"
end

desc "link vimrc to ~/.vimrc"
task :link_vimrc do
  %w[ vimrc gvimrc ].each do |file|
    dest = File.expand_path("~/.#{file}")
    unless File.exist?(dest)
      ln_s(File.expand_path("../#{file}", __FILE__), dest)
    end
  end
end

task :clean do
  system "git clean -dfx"
end

desc "Pull the latest"
task :pull do
  system "git pull"
end

task :default => [
  :link_vimrc,
  :update_docs
]

desc "Clear out all build artifacts and rebuild the latest Janus"
task :upgrade => [:clean, :pull, :default]

