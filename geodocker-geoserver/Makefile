BASE := $(subst -, ,$(notdir ${CURDIR}))
ORG  := geomesa
REPO := $(word 2, ${BASE})
IMG  := quay.io/${ORG}/${REPO}
GEOMESA_VERSION := 1.3.0-m3-SNAPSHOT
GEOMESA_DIST_TARBALL := geomesa-accumulo-dist_2.11-${GEOMESA_VERSION}-bin.tar.gz
GEOMESA_ACCUMULO_GS_INSTALL := geomesa-accumulo-gs-plugin_2.11-${GEOMESA_VERSION}-install.tar.gz

build: ${GEOMESA_ACCUMULO_GS_INSTALL}
	docker build -t ${IMG}:latest	.

.cache/${GEOMESA_DIST_TARBALL}:
	mkdir -p .cache \
        && cp ${HOME}/.m2/repository/org/locationtech/geomesa/geomesa-accumulo-dist_2.11/${GEOMESA_VERSION}/${GEOMESA_DIST_TARBALL} .cache

${GEOMESA_ACCUMULO_GS_INSTALL}: .cache/${GEOMESA_DIST_TARBALL}
	tar -zx --strip-components=3 \
		-f .cache/${GEOMESA_DIST_TARBALL} \
    geomesa-accumulo_2.11-${GEOMESA_VERSION}/dist/gs-plugins/${GEOMESA_ACCUMULO_GS_INSTALL}
	@touch -am ${GEOMESA_ACCUMULO_GS_INSTALL}

publish: build
	docker push ${IMG}:latest
	if [ "${TAG}" != "" -a "${TAG}" != "latest" ]; then docker tag ${IMG}:latest ${IMG}:${TAG} && docker push ${IMG}:${TAG}; fi

test: build
	echo "TODO: make test for the container"

clean:
	rm -rf .cache
	rm -rf ${GEOMESA_ACCUMULO_GS_INSTALL}