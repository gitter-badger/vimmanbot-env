source "https://rubygems.org"

gem "nokogiri", "1.6.3.1"

group :development do
  gem "vagrant",
  git: "https://github.com/mitchellh/vagrant.git", tag:"v1.6.5"
end

group :plugins do
  gem "ansible"
  gem "vagrant-hostsupdater"
  gem "vagrant-vbguest"
end

group :integration do
  gem "serverspec"
end
