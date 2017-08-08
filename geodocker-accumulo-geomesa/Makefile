BASE := $(subst -, ,$(notdir ${CURDIR}))
ORG  := geomesa
REPO := $(word 2, ${BASE})-$(word 3, ${BASE})
IMG  := quay.io/${ORG}/${REPO}
TAG := geomesa-1.3.2-accumulo-1.7.2
ACCUMULO_VERSION := 1.7.2
GEOMESA_VERSION := 1.3.2
DIST_TARBALL := tarballs/geomesa-accumulo-dist_2.11-${GEOMESA_VERSION}-bin.tar.gz
RUNTIME := geomesa-accumulo-distributed-runtime_2.11-${GEOMESA_VERSION}.jar

build: ${RUNTIME} dockerfile
	docker build \
                --build-arg GEOMESA_VERSION=${GEOMESA_VERSION} \
                --build-arg TAG=${TAG} \
                --build-arg ACCUMULO_VERSION=${ACCUMULO_VERSION} \
		-f target/Dockerfile \
                -t ${IMG}:${TAG} .

dockerfile:
	mkdir -p target
	sed 's/__TAG__/'"${TAG}"'/' Dockerfile.template > target/Dockerfile


${DIST_TARBALL}:
	(mkdir -p tarballs; cd tarballs ; curl -L -C - -O "https://repo.locationtech.org/content/repositories/geomesa-releases/org/locationtech/geomesa/geomesa-accumulo-dist_2.11/${GEOMESA_VERSION}/geomesa-accumulo-dist_2.11-${GEOMESA_VERSION}-bin.tar.gz")
#	(mkdir -p tarballs; cd tarballs ;  cp ${HOME}/.m2/repository/org/locationtech/geomesa/geomesa-accumulo-dist_2.11/${GEOMESA_VERSION}/geomesa-accumulo-dist_2.11-${GEOMESA_VERSION}-bin.tar.gz . )

${RUNTIME}: ${DIST_TARBALL}
	tar zxvf $<
	cp geomesa-accumulo_2.11-${GEOMESA_VERSION}/dist/accumulo/${RUNTIME} .

publish: build
	docker push ${IMG}:${TAG}

clean:
	rm -rf geomesa-accumulo_2.11-*
	rm -rf tarballs

cleanest: clean
	rm -f geomesa-accumulo-dist*
	rm -rf tarballs
