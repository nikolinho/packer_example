# packer_example

1.	Create a Packer file.
vi packer.json
2.	In the vi text editor, configure the following:
3.	{
4.	 "variables": {
5.	   "repository": "la/express",
6.	   "tag": "1.0"
7.	 },
8.	 "builders": [
9.	   {
10.	     "type": "docker",
11.	     "author": "Travis Thomsen",
12.	     "image": "node",
13.	     "commit": true,
14.	     "changes": [
15.	       "EXPOSE 3000",
           "ENTRYPOINT node /var/code/bin/www"
16.	     ]
17.	   }
18.	 ],
19.	 "provisioners": [
20.	   {
21.	     "type": "shell",
22.	     "inline": [
23.	       "apt-get update -y && apt-get install curl -y",
24.	       "mkdir -p /var/code",
25.	       "cd /root",
26.	       "curl -L https://github.com/linuxacademy/content-nodejs-hello-world/archive/v1.0.tar.gz -o code.tar.gz",
27.	       "tar zxvf code.tar.gz -C /var/code --strip-components=1",
28.	       "cd /var/code",
29.	       "npm install"
30.	     ]
31.	   }
32.	 ],
33.	 "post-processors": [
34.	   {
35.	     "type": "docker-tag",
36.	     "repository": "{{user `repository`}}",
37.	     "tag": "{{user `tag`}}"
38.	   }
39.	 ]
}
40.	Press ESC and type :wq to save the changes and exit the vi text editor.
41.	Validate the Packer file.
packer validate packer.json
Build a Docker Image
1.	Build the Docker image using the Packer file.
packer build --var 'repository=la/express' --var 'tag=0.0.1' packer.json
2.	Verify that the image was created.
docker images
Create a Docker Container
1.	Build the Docker container.
docker run -dt -p 80:3000 la/express:0.0.1 node /var/code/bin/www
2.	Verify that the Docker container started.
docker ps
3.	Verify that everything is working correctly.
curl localhost
Conclusion
Congratulations, you've successfully completed this hands-on lab!

