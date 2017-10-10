FROM debian:stretch-slim

RUN { set -ex; \
	# install deps
	apt-get update; \
	apt-get install --no-install-recommends -y libssl1.0.2; \
}

RUN { set -ex; \
	# install deps
	BUILD_DEPS="g++ build-essential curl git ca-certificates file libssl-dev pkg-config libudev-dev"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \
	curl https://sh.rustup.rs -sSf | sh -s -- -y; \
	export PATH=/root/.cargo/bin:${PATH}; \

	# show tool versions
	rustc -vV; \
	cargo -V; \
	gcc -v; \
	g++ -v; \

	# download
	git clone https://github.com/paritytech/parity /tmp/parity --depth 1 --branch v1.7.3; \
	cd /tmp/parity; \
	git checkout d097956ede0b05ffd29d6ccf4a4248bd90cee599; \

	# build & install
	export RUST_BACKTRACE=1; \
	cargo build --release --features final --verbose; \
	mv target/release/parity /usr/bin; \

	# clean up
	apt-get remove -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cargo /root/.rustup; \
}

ENTRYPOINT ["parity"]
