# Deploy Ruby app
> This ap is a Ruby app deployment report

## List of commands
```
terraform plan
terraform apply --auto-approve
ssh makuznet-at-gmail-com-ruby.devops.rebrain.srwx.net -l root
    apt update
    apt -y install curl git sudo nodejs
    adduser ruby # interactive Debian script using instead useradd command
    exit
ssh makuznet-at-gmail-com-ruby.devops.rebrain.srwx.net -l root
    usermod -aG sudo ruby
    getent group sudo
    su - ruby
    sudo echo "$USER ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee -a /etc/sudoers

    curl -sSL https://get.rvm.io | bash -s stable
    source /home/ruby/.rvm/scripts/rvm
    # mkdir /home/ruby/blog
    # cd /home/ruby/blog
        git clone https://github.com/1sherlynn/alphacamp_blog_app .
        cd blog/config # Required ruby-2.4.1 is not installed.
        rvm install ruby-2.4.1
        cd ~
    
    sudo apt -y install postgresql libpq-dev
    sudo -u postgres psql
        \password postgres # set a password for postgres user
        \q
    # cd /home/ruby/blog/config
                cp database.yml database.yml.original
                vi database.yml # change lite to postgresql        
            cd ..
        vi /home/ruby/.bashrc
            export PROJECT_DATABASE_PASSWORD=netlab 
        source /home/ruby/.bashrc      
        env | grep PROJECT_DATABASE_PASSWORD
        gem install execjs
        gem install unicorn
        cp Gemfile Gemfile.original
        vi Gemfile
            gem 'sqlite3' , group: :development # remove a line
            gem 'pg', '~> 0.18.2', group: :development # add a line
        bundle install # I didn't install Bundler
        sudo vi /etc/postgresql/11/main/pg_hba.conf
            local     all        postgres            peer  # remove
            local     all        postgres            md5   # add
        sudo systemctl restart postgresql
        bundle install --with Production
        vi config/environments/development.rb
            config.file_watcher = ActiveSupport::EventedFileUpdateChecker # comment this line
        bundle exec rake db:create
        bundle exec rake db:migrate
        vi db/seeds.rb
            # puts Seeding Post records # comment this line
        bundle exec rake db:seed # feel with test data
        bundle exec rails server
        sudo apt -y install nginx
        letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-uni.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos
        ansible-playbook -i inventory.yml main.yml # nginx.conf and unicorn.rb conf files
        unicorn -c /home/ruby/blog/config/unicorn.rb -D

            
            
           
```


## Usage

## Installation
### Video lesson
```bash
useradd -s /bin/bash -G sudo -m ruby
su - ruby
ruby -v
https://rvm.io
\curl -sSL https://get.rvm.io | bash -s stable
source /Users/makuznet/.rvm/scripts/rvm # To start using RVM you need to run `source /Users/makuznet/.rvm/scripts/rvm` in all your open shell windows
rvm install ruby-2.5
rvm use ruby-2.5.1

git clone https://github.com/qsherlynn/alphacamp_blog_app
cd alphacamp_blog_app
gem install bundle # gem is a Ruby packet manager; bundle is a packet manager that can read Gemfile
cat Gemfile
bundle install # installing all dependencies
cd config
cat database.yml # database connection rule as in PHP env and 
bundle exec rails routes
bundle exec rake db:migrate #    db and tables are created
bundle exec rails server # built-in RoR web server port: 3000

gem install --file Gemfile # the same as 'bundle install'
```

### Letsencrypt
```bash
sudo apt -y install letsencrypt

# getting an ssl certificate
letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-uni.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos

# backing up /etc/letsencrypt
scp -r root@makuznet-at-gmail-com-uni.devops.rebrain.srwx.net:/etc/letsencrypt ~/Documents/rebrain/deploy-js/

# restore
scp -r ~/Documents/rebrain/deploy-js/letsencrypt root@makuznet-at-gmail-com-ruby.devops.rebrain.srwx.net:/etc/
```











## Acknowledgments
This repo was inspired by [rebrainme.com](https://rebrainme.com) team

## See Also
-[rvm — Ruby Version Manager](https://rvm.io)
-[Changing sqlite to PostgreSQL in ruby on rails](https://stackoverflow.com/questions/50311186/changing-sqlite-to-postgresql-in-ruby-on-rails)
-[Peer authentication failed for user “postgres”](https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge)
-[Rails bundle install production only](https://stackoverflow.com/questions/10912614/rails-bundle-install-production-only)
-[LoadError: Could not load the 'listen' gem (Rails 5)](https://stackoverflow.com/questions/38663706/loaderror-could-not-load-the-listen-gem-rails-5)
-[Unicorn config files examples](https://yhbt.net/unicorn/examples/)
-[]()
-[]()
-[]()
-[]()
-[]()
-[]()

## License
Follow all involved parties licenses terms and conditions.