# install command
<pre>mkdir ~/greenlight && cd ~/greenlight
docker run --rm bigbluebutton/greenlight:v2 cat ./sample.env > .env
docker run --rm bigbluebutton/greenlight:v2 bundle exec rake secret`
sudo bbb-conf --secret
echo SECRET_KEY_BASE
echo BIGBLUEBUTTON_ENDPOINT
echo BIGBLUEBUTTON_SECRET
docker run --rm --env-file .env bigbluebutton/greenlight:v2 bundle exec rake conf:check
docker run --rm bigbluebutton/greenlight:v2 cat ./greenlight.nginx | sudo tee /etc/bigbluebutton/nginx/greenlight.nginx
sudo systemctl restart nginx
cd ~/greenlight
docker-compose -v
docker run --rm bigbluebutton/greenlight:v2 cat ./docker-compose.yml > docker-compose.yml
export pass=$(openssl rand -hex 8); sed -i 's/POSTGRES_PASSWORD=password/POSTGRES_PASSWORD='$pass'/g' docker-compose.yml;sed -i 's/DB_PASSWORD=password/DB_PASSWORD='$pass'/g' .env
docker-compose up -d
</pre>

# add admin
<pre>docker exec greenlight-v2 bundle exec rake user:create["strongpapazola","example@gmail.com","123456789","admin"]</pre>

# restart greenlight
<pre>
cd ~/greenlight
docker-compose down
./scripts/image_build.sh <name_image> <release_name>
docker-compose up -d
</pre>
