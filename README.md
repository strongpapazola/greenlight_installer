# <strong>{DEVELOPING}</strong>

# Install Command
1. Install Docker
<pre>docker -v</pre>

2. Install greenlight
<pre>git clone https://github.com/bigbluebutton/greenlight.git</pre>

3. Configure Greenlight
<pre>cp sample.env .env</pre>
- Get Secret
Place on <strong>SECRET_KEY_BASE</strong> in .env
<pre>docker run --rm bigbluebutton/greenlight:v2 bundle exec rake secret</pre>
Place on <strong>BIGBLUEBUTTON_ENDPOINT</strong> and <strong>BIGBLUEBUTTON_SECRET</strong> in .env
<pre>sudo bbb-conf --secret</pre>
- Setting Allowed Hosts
Edit on SAFE_HOSTS to your domain
<pre>SAFE_HOSTS=bbb.example.com</pre>
- Verifying Configuration
docker run --rm --env-file .env bigbluebutton/greenlight:v2 bundle exec rake conf:check

4. Configure Nginx to Route To Greenlight
<pre>cat ./greenlight.nginx | sudo tee /etc/bigbluebutton/nginx/greenlight.nginx
sudo systemctl restart nginx</pre>

5. Start Greenlight 2.0
<pre>docker-compose -v
docker run --rm bigbluebutton/greenlight:v2 cat ./docker-compose.yml > docker-compose.yml
export pass=$(openssl rand -hex 8); sed -i 's/POSTGRES_PASSWORD=password/POSTGRES_PASSWORD='$pass'/g' docker-compose.yml;sed -i 's/DB_PASSWORD=password/DB_PASSWORD='$pass'/g' .env</pre>
Next, in the docker-compose.yml file, replace:
<pre>services:
  app:
    entrypoint: [bin/start]
    image: bigbluebutton/greenlight:v2</pre>
With:
<pre>services:
  app:
    entrypoint: [bin/start]
    image: {image name}:release-v2</pre>
  
</pre>./scripts/image_build.sh {image name} release-v2
docker-compose up -d</pre>

# Add Admin
<pre>docker exec greenlight-v2 bundle exec rake user:create["strongpapazola","example@gmail.com","123456789","admin"]</pre>

# Restart Greenlight
<pre>
cd ~/greenlight
docker-compose down
./scripts/image_build.sh {name_image} {release_name}
docker-compose up -d
</pre>

# <strong>after restart...you must wait 3 minute...if not you will got 404 error not found<strong>
