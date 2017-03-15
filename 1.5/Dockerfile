FROM debian:jessie

ENV RUST_BACKTRACE 1

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
	curl https://static.rust-lang.org/rustup.sh | sh -s -- -y --prefix=/tmp/rust --disable-sudo; \
	export PATH=/tmp/rust/bin:${PATH}; \

	# show tool versions
	rustc -vV; \
	cargo -V; \
	gcc -v; \
	g++ -v; \

	# download
	git clone https://github.com/ethcore/parity /tmp/parity --depth 1 --branch v1.5.11; \
	cd /tmp/parity; \
	git checkout f067f12e99be8b285b9ab8c178e5dc38d096f047; \

	# build & install
	cargo build --release --verbose; \
	mv target/release/parity /usr/bin; \

	# clean up
	apt-get remove -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cargo; \
}

ENTRYPOINT ["parity"]
