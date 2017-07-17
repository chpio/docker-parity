FROM debian:jessie-slim

RUN { set -ex; \
	# install deps
	apt-get update; \
	apt-get install --no-install-recommends -y libssl1.0.0; \
}

RUN { set -ex; \
	# install deps
	BUILD_DEPS="g++ build-essential curl git ca-certificates file libssl-dev pkg-config"; \
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
	git clone https://github.com/paritytech/parity /tmp/parity --depth 1 --branch v1.5.13; \
	cd /tmp/parity; \
	git checkout 69efb2743200817c6640db1d07583398b0da2237; \

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
