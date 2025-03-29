# Installing Ruby Versions on M Series Macs

(AKA: Apple Silicon)

## Use rbenv

(Install via Homebrew if you don't already have it - `brew install rbenv`)


### Install your rubies

Set the following before you start:
```
export optflags="-Wno-error=implicit-function-declaration"
```

#### The new stuff

| Version | Install command |
| ------  | --------------- |
| 3.1.0   | rbenv install 3.1.0 -- --with-openssl-dir=$(brew --prefix)/openssl@3.0 |

#### The easy ones

| Version | Install command |
| ------  | --------------- |
| 3.0.0   | rbenv install 3.0.0 -- --with-openssl-dir=$(brew --prefix)/openssl@1.1 |
| 2.6.7   | rbenv install 2.6.7 -- --with-openssl-dir=$(brew --prefix)/openssl@1.1 |
| 2.5.9   | rbenv install 2.5.9 -- --with-openssl-dir=$(brew --prefix)/openssl@1.1 |
| 2.4.10  | rbenv install 2.4.10 -- --with-openssl-dir=$(brew --prefix)/openssl@1.1 |

#### The OpenSSL 1.0 era

If you don't already have openssl 1.0, it's been disabled in Homebrew so we're going to need to install a patched copy before we can use it to compile rubies. I got mine from https://github.com/aarishgilani/openssl-1.0 (and I have a fork of it, in the unlikely event that it disappears)

Download the `openssl@1.0.rb` file and install it into Homebrew by doing this:
```
brew install ~/Downloads/openssl@1.0.rb
```

| Version | Install command |
| ------  | --------------- |
| 2.3.8   | rbenv install 2.3.8 -- --with-openssl-dir=$(brew --prefix)/openssl@1.0 |


#### The ones that need to be hacked 

Not only do these require openssl 1.0 but they also don't compile cleanly so there's some extra faff involved. Works for versions 2.0.0 thru 2.2.10

Install "as usual"…
```
rbenv install 2.1.10 -- --with-openssl-dir=$(brew --prefix)/openssl@1.0
```
…and wait for it to break with something like this:
> BUILD FAILED (macOS 14.6.1 on arm64 using ruby-build 20250318)
>
> You can inspect the build directory at /var/folders/gl/dtfc853x6pz368yh8xg8kj080000gn/T/ruby-build.20250329024757.97115.MFZfoS
>

* `cd` into the "build directory" folder, then into the subfolder of the ruby directory e.g. `ruby-2.1.10/`
* Edit the `config.status` file, search for "`*) rm -f`"
* Replace the offending line with `*)  mv "$ac_tmp/out" "$ac_file";;`
* Save and exit the file
* run `make && make install`

(adapted from https://github.com/rbenv/ruby-build/issues/1742#issuecomment-2133592995)

&nbsp;
    
Thanks to [@lopespm](https://github.com/@lopespm), [@aarishgilani](https://github.com/@aarishgilani) and [@felixbuenemann](https://github.com/felixbuenemann) whose work this install guide is based on
