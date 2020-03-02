mac pip3 ssl module not avalible
2018.01.16 10:41:28

brew reinstall openssl 

解决ssl无法link的问题
brew update
brew install openssl
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/Cellar/openssl/1.0.2j/bin/openssl /usr/local/bin/openssl

brew reinstall python3

现在pip3应该已经可以用了。

In Mac OS X, to solve the Python error “ValueError: unknown locale: UTF-8“:
Add some lines to your ~/.bash_profile then re-start bash (or re-login):

export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

alter user opening superuser createrole createdb replication;