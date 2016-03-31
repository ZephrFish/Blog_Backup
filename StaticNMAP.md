# How to Statically Compile NMAP
Here is a short explanation on how to compile nmap statically. 

Version of nmap used: https://nmap.org/dist/nmap-7.11.tar.bz2

## Method 
First set up the environment (''-fPIC'' is needed, for static compilation):


    export CFLAGS="-march=core2 -O2 -fomit-frame-pointer -pipe -fPIC"
    export CXXFLAGS="-march=core2 -O2 -fomit-frame-pointer -pipe -fPIC"



Then run ./configure with minimal options:



    ./configure --without-subversion --without-liblua --without-zenmap --with-pcre=/usr --with-libpcap=included --with-libdnet=included --without-ndiff --without-nmap-update --without-ncat --without-liblua --without-nping --without-openssl



And compile - this will mostly work:



    make -j4 static



The final compilation step fails - the actual error is: `/usr/lib/gcc/x86_64-unknown-linux-gnu/4.8.4/../../../../x86_64-unknown-linux-gnu/bin/ld: dynamic STT_GNU_IFUNC symbol `strcmp' with pointer equality in `/usr/lib/gcc/x86_64-unknown-linux-gnu/4.8.4/../../../../lib/libc.a(strcmp.o)' can not be used when making an executable; recompile with -fPIE and relink with -pie`

## Making it Work
Hacking the Makefile, to [make the compilation](http://stackoverflow.com/questions/26277283/gcc-linking-libc-static-and-some-other-library-dynamically-revisited) ''mostly'' static:

1. Change the `LIBS =  -lnsock -lnbase -lpcre` line 54 to:

    LIBS =  -lnsock -lnbase $(LIBPCAPDIR)/libpcap.a $(OPENSSL_LIBS) libnetutil/libnetutil.a $(top_srcdir)/libdnet-stripped/src/.libs/libdnet.a  $(top_srcdir)/liblinear/liblinear.a -ldl

2. Change the `$(CXX) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)` line 122 (under "Compiling nmap") to:

    $(CXX) $(LDFLAGS) -o $@ $(OBJS) $(LIBS) /usr/lib/libpcre.a


And re-run the final compilation step:


    make


This succeeds, and we have:


    $ ldd nmap | cut -d '(' -f1
    	linux-vdso.so.1 
    	libdl.so.2 => /usr/lib/libdl.so.2 
    	libstdc++.so.6 => /usr/lib/libstdc++.so.6 
    	libm.so.6 => /usr/lib/libm.so.6 
    	libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 
    	libc.so.6 => /usr/lib/libc.so.6 
    	/lib64/ld-linux-x86-64.so.2

This is as ''static'' as the executable can be made, with glibc - those are all links to glibc's libraries.


