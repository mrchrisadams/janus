vim_plugin_task "tinymode.vim",     "git://github.com/vim-scripts/tinymode.vim.git"
vim_plugin_task "jslint",           "git://github.com/hallettj/jslint.vim.git"
# vim_plugin_task "tabular"           "git://github.com/godlygeek/tabular.git"
vim_plugin_task "yankring-vim",     "git://github.com/chrismetcalf/vim-yankring.git"
vim_plugin_task "scratch",          "git://github.com/kana/vim-scratch.git"
vim_plugin_task "loremipsum",       "git://github.com/tomtom/vimtlib.git"
vim_plugin_task "confluencewiki",   "git://github.com/vim-scripts/confluencewiki.vim.git"
vim_plugin_task "peepopen",         "git://github.com/mrchrisadams/vim-peepopen.git"

vim_plugin_task "IR_white" do
  sh "curl https://raw.github.com/squil/vim_colors/04d696a1d16a934c13bd578bc0e5dab5afb7e903/IR_White.vim  > colors/IR_White.vim"
end

# remove plugins we don't want
# TODO redefine gvimrc and vimrc to remove bindings too
needless_plugins = %w( color_sampler conque vividchalk zoomwin puppet command_t)

needless_plugins.each do |plugin|
  skip_vim_plugin plugin
end
