# A sample Guardfile
# More info at https://github.com/guard/guard#readme

guard 'jekyll', localeditmode: true, guardscript: "http://0.0.0.0:4444/livereload.js" do
  watch /.*/
  ignore /_site/
end

guard 'livereload', host: '0.0.0.0', port: '4444' do
  watch /.*/
end
