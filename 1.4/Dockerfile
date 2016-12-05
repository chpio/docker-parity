FROM debian:jessie

ENV RUST_BACKTRACE 1

RUN { set -ex; \
	# install deps
	BUILD_DEPS="g++ build-essential curl git ca-certificates file"; \
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
	git clone https://github.com/ethcore/parity /tmp/parity --depth 1 --branch v1.4.6; \
	cd /tmp/parity; \
	git checkout 5feccf11d2156e3444f1c7dbacc76212ff06d319; \

	# build & install
	cargo build --release --verbose; \
	mv target/release/parity /usr/bin; \

	# clean up
	apt-get remove -y $BUILD_DEPS; \
	apt-get autoremove -y; \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cargo; \
}

ENTRYPOINT ["parity"]
