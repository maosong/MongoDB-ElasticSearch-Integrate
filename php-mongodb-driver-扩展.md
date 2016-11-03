# PHP MongoDB Driver 扩展

PHP7 之前我们通过 pecl 扩展 [mongo](http://pecl.php.net/package/mongo) 访问MongDB，但这个库目前已经停止维护并且仅支持PHP5.x。原作者开发了 [mongodb](http://pecl.php.net/package/mongodb) 取而代之，同时支持PHP5.x和PHP7，考虑到后续向PHP7的迁移，我们必须使用新的类库。

## 安装

$ versionPhpExt=1.1.9

$ wget http:\/\/pecl.php.net\/get\/mongodb-**$**{versionPhpExt}.tgz

$ tar -zxf mongodb-**$**{versionPhpExt}.tgz

$ rm -f mongodb-**$**{versionPhpExt}.tgz

$ rm -f package\*.xml

$ cd mongodb-**$**{versionPhpExt}

$ phpize

$ .\/configure

$ make -j 8

$ make install

$ cd ..

$ rm -Rf mongodb-**$**{versionPhpExt}

$ echo 'extension=mongodb.so' &gt; \/etc\/php\/mods-available\/mongodb.ini



