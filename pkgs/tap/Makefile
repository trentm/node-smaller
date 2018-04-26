BUILD := build
MINI_VERSION := $(json -f ../../package.json version)
MINI_AUTHOR := $(json -f ../../package.json author)

all: clean src hack

clean:
	rm -rf $(BUILD)

src:
	git clone https://github.com/tapjs/node-tap.git $(BUILD)/src

hack:
	(cd $(BUILD)/src && git checkout package.json)
	json -If $(BUILD)/src/package.json -e "
		this.name = '@mini/' + this.name;
		this.version = this.version + '-$(MINI_VERSION)';
		this.author = '$(MINI_AUTHOR)';
		// Drop these large deps for coverage support.
		delete this.dependencies['nyc'];
		delete this.dependencies['coveralls'];
	"
	(cd $(BUILD)/src
		&& git checkout bin/run.js
		&& git apply ../../optional-import.patch)
