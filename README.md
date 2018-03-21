# k8s-images

Image build pipeline, mostly, for k8s workloads.

Available at: https://hub.docker.com/r/apealive

## Install

TODO: rewrite to https://github.com/pypa/pipenv

We use virtualenv to isolate python requirements to build images.

Prerequsites:

    python -m ensurepip --default-pip
    pip install --user virtualenv
    PYENV=~/.pyenv_dockermake
    python -m virtualenv -p python ${PYENV}

    . ${PYENV}/bin/activate

DockerMake (https://github.com/avirshup/DockerMake):

    pip install DockerMake
    # or for latest
    pip install -e "git+https://github.com/avirshup/DockerMake#egg=dockermake"


## Usage

To list and build targets:

	docker-make --list
	docker-make [target]


Full spec.:

	docker-make [-h] [-f MAKEFILE] [-a] [-l] [--build-arg BUILD_ARG]
			   [--requires [REQUIRES [REQUIRES ...]]] [--name NAME] [-p]
			   [-n] [--dockerfile-dir DOCKERFILE_DIR] [--pull]
			   [--cache-repo CACHE_REPO] [--cache-tag CACHE_TAG]
			   [--no-cache] [--bust-cache BUST_CACHE] [--clear-copy-cache]
			   [--keep-build-tags] [--repository REPOSITORY] [--tag TAG]
			   [--push-to-registry] [--registry-user REGISTRY_USER]
			   [--registry-token REGISTRY_TOKEN] [--version] [--help-yaml]
			   [--debug]
			   [TARGETS [TARGETS ...]]

Workflow:

        # optional
        TAG="--tag $(date '+%Y%m%d')"
        PUSH="--push-to-registry -r docker.io/epcim --registry-user XXX --registry-token XXX"
        CACHE="--no-cache"

        docker-make --list
        docker-make [TARGET] $TAG
        docker-make --all    $TAG

        # to build all targets (that has FROM* specified):
        docker-make --list | awk '/*/{print $2}'| grep -v '^base\|^common\|^tini' | xargs -n1 -I% docker-make %  $TAG $PUSH $CACHE



## Other

For advanced automation integrate with:
* pyInvoke (https://github.com/pyinvoke/invoke)
