FROM debian:jessie-slim

ENV RUST_BACKTRACE 1

RUN { set -ex; \
	# install deps
	apt-get update; \
	apt-get install --no-install-recommends -y libssl1.0.0; \
}

RUN { set -ex; \
	# install deps
	BUILD_DEPS="g++ build-essential curl git ca-certificates file libssl-dev pkg-config libudev-dev"; \
	apt-get update; \
	apt-get install --no-install-recommends -y $BUILD_DEPS; \
	curl https://sh.rustup.rs -sSf | sh -s -- -y --default-toolchain 1.16.0; \
	export PATH=/root/.cargo/bin:${PATH}; \

	# show tool versions
	rustc -vV; \
	cargo -V; \
	gcc -v; \
	g++ -v; \

	# download
	git clone https://github.com/paritytech/parity /tmp/parity --depth 1 --branch v1.6.6; \
	cd /tmp/parity; \
	git checkout 8c6e3f314a60b6ba42dddc8e7485b891bffbfe96; \

	# build & install
	cargo build --release --verbose; \
	mv target/release/parity /usr/bin; \

	# clean up
	apt-get remove -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cargo /root/.rustup; \
}

ENTRYPOINT ["parity"]
