# Copyright 2024 Linus Arver
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# NOTE: Below is an installation rule for a Nix + Cabal workflow. It may be
# useful to do this in case stack doesn't work for some reason. Originally we
# wanted to use this workflow instead but ran up against a Nix + MacOS breakage
# (https://github.com/NixOS/nixpkgs/pull/149942). Currently only stack works, so
# that's what we use. It may be the case though that we end up using stack
# long-term, because of its stability guarantees.
#
#       MELBY_PROJECT_ROOT=$(PWD) cabal install --overwrite-policy=always

# Let stack be aware of NixOS's non-standard library paths. This is an
# alternative to specifying the `nix:` field in stack.yaml directly (we just
# want to use a plain stack.yaml that is not aware of NixOS so that we can use
# the same stack.yaml file on non-NixOS distributions including MacOS).
ZLIB_SO_DIR=$(shell pkg-config --libs-only-L zlib)
ZLIB_H_DIR=$(shell pkg-config --cflags-only-I zlib)

all: melbyc

melbyc:
	MELBY_PROJECT_ROOT=$(PWD) stack build \
		--copy-bins \
		--local-bin-path $(PWD) \
		--no-nix-pure \
		--extra-lib-dirs=$(ZLIB_SO_DIR:-L%=%) \
		--extra-include-dirs=$(ZLIB_H_DIR:-I%=%)
.PHONY: melbyc

ghci:
	stack exec -- ghci
.PHONY: ghci
