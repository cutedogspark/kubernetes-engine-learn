image-v1:
	docker build -t gcr.io/${PROJECT_ID}/hello-web:v1 \
		--build-arg VERSION=1.0.0 .

image-v2:
		docker build -t gcr.io/${PROJECT_ID}/hello-web:v2 \
		--build-arg VERSION=2.0.0 .

push-image-v1:
	docker push gcr.io/${PROJECT_ID}/hello-web:v1

push-image-v2:
	docker push gcr.io/${PROJECT_ID}/hello-web:v2