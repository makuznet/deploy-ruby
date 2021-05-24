# Deploy Ruby app
> This ap is a Ruby app deployment report

## Usage  
### List of commands
```
terraform init
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

    \curl -sSL https://get.rvm.io | bash -s stable
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
        env | grep PROJECT_DATABASE_PASSWORD
        gem install execjs
        gem install unicorn
        cp Gemfile Gemfile.original
        vi Gemfile
            gem 'sqlite3' , group: :development # remove a line
            gem 'pg', '~> 0.18.2', group: :development # add a line
            gem 'execjs'
            gem 'unicorn'
        bundle install
        sudo vi /etc/postgresql/11/main/pg_hba.conf
            local     all        postgres            peer  # remove
            local     all        postgres            md5   # add
        sudo systemctl restart postgresql
        bundle install --with Production
        bundle exec rake db:create
        bundle exec rake db:migrate
        vi db/seeds.rb
            # puts Seeding Post records # comment this line
        bundle exec rake db:seed # feel with test data
        bundle exec rails server # check that the app works
        sudo apt -y install nginx letsencrypt
        letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-uni.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos
ansible-playbook -i inventory.yml main.yml # copying config and service files        
```
### Stop Unicorn processes manually
```bash
ps -aux | grep unicorn
kill -9 21737 # 21737 — master process id
```
### Start Unicorn manually
```bash
unicorn -c /home/ruby/blog/config/unicorn.rb -D
```

## Installation
### Ruby Version Manager
```bash
\curl -sSL https://get.rvm.io | bash -s stable
```
### Letsencrypt
```bash
sudo apt -y install letsencrypt
```
# getting an ssl certificate
```bash
letsencrypt certonly --webroot -w /var/www/html -d makuznet-at-gmail-com-uni.devops.rebrain.srwx.net -m makuznet@gmail.com --agree-tos
```
# backing up /etc/letsencrypt
```bash
scp -r root@makuznet-at-gmail-com-uni.devops.rebrain.srwx.net:/etc/letsencrypt ~/Documents/rebrain/deploy-js/
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
-[Unicorn systemd service](https://www.ralfebert.de/tutorials/rails-deployment/)  

## License
Follow all involved parties licenses terms and conditions.