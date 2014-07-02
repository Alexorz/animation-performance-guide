desc 'start server'
task 'default' do
  fork { sh 'guard' }
  fork { sh 'jekyll serve --watch' }
  Process.waitall
end
