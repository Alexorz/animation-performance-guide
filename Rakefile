desc 'start server'
task 'default' do
  fork { sh 'jekyll serve -w' }
  fork { sh 'guard' }
  Process.waitall
end
