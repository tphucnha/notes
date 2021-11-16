See https://zwbetz.com/install-sqlplus-on-linux/

Get error about libaiso, see https://stackoverflow.com/questions/10619298/libaio-so-1-cannot-open-shared-object-file

Some env should be aware of:
```
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

#(This one is important)
ENV NLS_LANG=.AL32UTF8
```
