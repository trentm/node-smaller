BUILD := build
PACKAGE_JSON := $(BUILD)/src/package.json

all: clean src hack publish

clean:
	rm -rf $(BUILD)

src:
	git clone https://github.com/tapjs/node-tap.git $(BUILD)/src

hack:
	cp README.md $(BUILD)/src/
	smallerVersion="$(shell json -f ../../package.json version)" && \
		smallerAuthor="$(shell json -f ../../package.json author)" && \
		cd $(BUILD)/src && \
		git checkout package.json && \
		json -If package.json -e " \
			this.name = '@smaller/' + this.name; \
			this.version = this.version + '-$$smallerVersion'; \
			this.author = '$$smallerAuthor'; \
			this.homepage = 'https://github.com/trentm/node-smaller/tree/master/pkgs/tap'; \
			delete this.dependencies['nyc']; \
			delete this.dependencies['coveralls']; \
			delete this.repository; \
			delete this.scripts.postpublish; \
		"
	cd $(BUILD)/src \
		&& git checkout bin/run.js \
		&& git apply ../../optional-import.patch

publish:
	[[ -z `git status --short` ]]  # If this fails, the working dir is dirty.
	ver="$(shell json -f $(PACKAGE_JSON) version)" && \
		name="$(shell json -f $(PACKAGE_JSON) name)" && \
		publishedVer=$(shell npm view -j "$(shell json -f $(PACKAGE_JSON) name)@$(shell json -f $(PACKAGE_JSON) version)" version 2>/dev/null) && \
		if [[ -n "$$publishedVer" ]]; then \
			echo "$$name@$$ver is already published to npm"; \
			exit 0; \
		fi && \
		echo "** Are you sure you want to tag and publish $$name@$$ver to npm?" && \
		echo "** Enter to continue, Ctrl+C to abort." && \
		read && \
		cd $(BUILD)/src && \
		npm publish --access=public
