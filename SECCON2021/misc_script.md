# s/<script>//gi

## Introduction
This is my first writeup for CTF challenge in my life.
I participated SECCON 2021 the most popular CTF challenge in Japan.
I could solve a problem "s/<script>//gi"
So I will share my solution for this problem.

## Problem

    Can you figure out why s/<script>//gi is insufficient for sanitizing? This can be bypassed with <scr<script>ipt>.
    Remove <script> (case insensitive) from the input until the input contains no <script>.
    Note that flag format is SECCON{[\x20-\x7e]+}, which means that the flag may contains < or > as the following examples.
    Sample Input 1:
    S3CC0N{dum<scr<script>ipt>my}
    Sample Output 1:
    S3CC0N{dummy}
    Sample Input 2 (small.txt):
    S3CC0N{dumm<scrIpT>y_flag>_<_pt>>PT><<SCr<S<<SC<SCRIpT><scRiPT>Ript>sCr<Scri<...
    Sample Output 2:
    S3CC0N{dummy_flag>_<_pt>>PT><sCRIp<scr<scr<scr!pt>ipt>ipt>}

And a file named "flag.tar.gz" was attached.
In this file, a file named "small.txt" and a file named "flag.txt" were contained.

    $ ls -l flag.txt small.txt 
    -rwxrwxr-x 1 yuno yuno 67108968 Oct 30 20:16 flag.txt
    -rw-r--r-- 1 yuno yuno     3276 Nov 12  1995 small.txt

The size of "small.txt" is 3,276 byte, on the other hand the size of flag.txt is 67,108,968 byte very larger than "small.txt".
The content of "small.txt" is as follows.

    $ cat small.txt 
    S3CC0N{dumm<scrIpT>y_flag>_<_pt>>PT><<SCr<S<<SC<SCRIpT><scRiPT>Ript>sCr<Scri<sCRIpT><ScRIPt>p<ScripT>T>iP<sCRIpt>T>cr<scRiPT>IPt>i<sC<S<ScR<s<SCrIpT>CRIpT>Ipt<sCrIPt>><ScRIPT><s<ScrIPt>CRIpt>c<sC<scRiPt>RIPT<scrIPT>>RiPT><<sCRipt>scrIPT<sCr<sCRiPt>IPt>>RipT><SC<<S<<ScRi<ScR<Script>iPT>pt>sCRIPT>cr<<Scr<<SCRipt>scRipt><SCrIPT>iPt>SCRI<scRiPt>Pt<sc<SCriPT>riPt>>Ipt>S<SCRiP<scRipT><ScRIPt>T<ScRIP<SC<<SCRIpt>scripT>ri<sCrIpt>pT><S<sCRIPt>crIPt>t>><<sCriPt>sCRIPT>cRIpt><<SCRiPT>SCRIpt>rIPT<<sCRIPT>SCr<Scri<sCRIPT><<SCRIPT>scRiPT>Pt><SCriPt>I<S<S<SCRiPt>C<<sCRIpt>S<SCriPt><scrIPT>CrIpT><sCRiPT>r<S<ScripT>cript>IPT>c<SCrIpt>RiPt><ScrIpt><S<<ScRiPt>scrIpT>cRIPt><s<<ScrIpt>scrip<sCR<<scRiPT>ScRiPt>ipT><sCRIpT><ScRipT>T><<scRiPt>sCRiPt>Crip<Scr<SCrIpt><SCrIP<ScriPt>t>Ipt>T<ScrIp<sCrIpt<SC<SCRI<ScRiPT>pT>RIp<ScRIpT>T>>T>><SCr<sCriPT><SCrIPT>I<SCrIpT><SCrIpT<sCRIPt<sCRipT>>>pT><sCrI<SCRiPT><<SCRipt>sCr<sCRi<sCRiPt<scriPt>>pT>IPT><scriP<S<scRIPT>CrIPT>T>pT><SCRIpt>p<S<ScrIpt>cr<Sc<SCrIpT>RIPT>iPt><sCrIPt>T<<ScRipt>scrIPt>><s<Scri<sCrIP<sCRipt>t>pT><scRipT>cRIPt>><scRiPT><ScriPT><scRipT>P<scRipt>T><Scr<<sCrIPt>sCrIPT>i<SCrIp<scRIPT>T>p<SCRiPt><ScRiPT><ScRIPt><ScRIpt>T<sc<ScRipt>ripT>><ScrIp<SCript>T><scRiPT><sCrIPt>sCRIp<scr<scr<<<SC<SCR<Scri<<<scripT<scrIp<<SCri<scRiPT<SCRi<ScR<Scrip<ScriP<ScriP<s<SC<sC<<sCrip<sCriPT<<<scrIP<scrI<s<SCri<scRi<SCr<SC<sCriPT<ScrI<SCrI<SC<sc<ScR<ScRIPT<S<ScriP<scrIpt>T>CRIpT>>Ipt>RIpT>RIpt>PT>PT>>ript>IPT>pT>Pt>CriPt>Pt>t>SCRIpT>ScrIPt>>t>SCRIPT>RIpT>Ript>CriPt>t>T>t>ipt>pT>>pT>SCript>T>>sCRiPT>scRiPt>pt>Ipt>RIpT>sCriPt>scr!pt><Sc<s<<sC<sCr<<SCRI<ScRIp<scRip<scri<ScRIp<ScRIp<<SC<scR<SCr<ScRi<sCRI<<sCrIPt<sc<sc<ScR<<<SC<S<SCRI<sC<scrIPt<sCR<sCri<ScripT<ScRiP<scriP<scR<scRipt<SCRipt<SCript<<<sCRiP<scrI<sc<ScRipt<sCriPt<scriPT<sCRI<SCr<sC<s<ScrIpT<ScRIP<SCRIpt<<SCRIpt<S<sc<SCrip<scR<ScrIp<<<SCr<<sCr<sCriP<SCRIP<sC<sc<Scr<Sc<sCrIP<ScRiPt<s<scr<sCRiP<sC<ScrIP<<sCRip<scRi<SCri<SCr<scrip<s<S<sc<<sC<SCr<SCRiPt<sCRIp<Sc<S<s<<SCr<<sCr<sCRI<<<scRi<s<SCr<Sc<scRI<<<ScRi<scRiP<SCrI<ScR<<sc<s<<S<SC<sCRIpT<SCrIPT<SCRIPt<Sc<<SCrip<SCR<ScRIP<ScRIPT<ScrIP<ScRIP<Scri<<<s<s<<s<ScR<sCRi<<s<<ScriPT<SCRIpt<SCr<scrip<scrIP<SCrI<SC<scr<Scr<scr<SC<SCRIpt<sCRip<<sC<<SCRIP<<<scR<SC<ScrIp<scrI<<<Scr<S<scr<Scr<SCr<scrip<ScrIpT<ScRIP<sc<s<scr<SCRIP<Scr<scrip<scr<scRi<SCrI<<<scr<sCRIP<Sc<S<SCr<sCr<sC<scRiPT<S<sC<scrIpT<scRiP<S<SCRipt>CrIpT>T>>rIpt>criPt>>ript>IPt>IpT>CripT>rIpt>T>iPt>ScriPt>ScRipT>pt>Pt>ipT>t>IPT>t>IPt>cRiPT>RipT>t>>T>IPt>iPT>IPT>crIPt>Ipt>SCRiPt>SCripT>pt>t>ript>IPT>SCrIpT>sCRIPt>t>sCRiPt>ripT>SCrIPT>t>>RiPT>IpT>iPT>iPt>RiPt>Pt>T>T>iPT>>>SCrIpT>cRiPt>SCRIPT>pT>IpT>cRiPT>sCRIpt>CRIPT>CRIpT>scrIpt>ScRiPt>pt>t>T>>T>iPt>t>scRiPt>RiPt>>>>rIpt>CriPT>SCRIPt>CRIPT>rIpT>scRiPt>IpT>Pt>T>Pt>sCRIpT>scriPT>pt>rIpt>IPt>CRIPT>pt>ScRiPT>ScRIpT>PT>IpT>ScRipt>Ipt>sCRIPt>cRipt>Cript>ripT>t>>iPt>Ript>SCRiPT>riPT>criPT>criPT>t>ipt>pt>PT>T>SCRIpt>T>Ript>T>iPT>cRIPt>>T>RIpT>IpT>RiPT>ripT>t>T>ipT>scrIPt>ipT>SCripT>SCrIpt>t>ipT>t>rIpt>CRiPT>>SCrIpT>>T>>cRiPt>rIPT>Ipt>PT>>>>rIPt>PT>t>ScRIpT>SCRipT>>>>ipT>T>T>>pt>Ipt>>ripT>pT>CriPt>rIpT>ScRIPt>sCRIPT>ipT>rIPt>RIPT>>ScRipT>Pt>Pt>ipT>IPT>rIPt>ScRIPt>T>t>pT>T>T>pT>ScRiPt>IPT>RIPt>sCrIpT>CRIPT>rIpT>ip<SCrIP<ScriPT><ScripT>T><sC<ScRi<scri<<sCript>scRiPT>pT>pT>R<SC<scriPT>rIPt><ScrIpT>Ipt>t>ipt>}

## My solution

As this problem, we must remove "<script>" from a file again and again.
In my solution, firstly, read characters one by one.
For deleting "<script>", when you read a character '>' then compare strings before '>' and "<script>" in case insensitive,
and if they matched, delete string "<script>".
After that, continue read characters, and return first, continue until EOF.  
If you do above, in this exported file, no "<script>" are contained.

In C language, a code is as follows.

    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>

```
int
    main( int argc, char **argv )
{
    if( argc < 2 )
    {
        return	-1;
    }

    char	*stack = (char*)malloc( 1280000000 );
    size_t	idx = 0, word_idx;

    char	word[] = "<SCRIPT>";
    int	wordlastidx = strlen( word ) - 1;

    FILE	*fp = fopen( argv[1], "rb" );
    int	c;
    while( ( c = fgetc( fp ) ) != EOF )
    {
        stack[idx] = c;
        idx++;

        if( c == word[wordlastidx] && idx > wordlastidx )
        {
            stack[idx] = '\0';
            if( strcasecmp( word, &stack[idx - wordlastidx - 1] ) == 0 )
            {
                idx -= wordlastidx + 1;
            }
        }
    }
    stack[idx] = '\0';

    printf( "%s\n", stack );

    free( stack );

    return	0;
}
```
   

    time ./rmscript flag.txt 
    SECCON{sanitizing_is_not_so_good><_escaping_is_better_iPt><SCript<ScrIpT<scRIp<scRI<Sc<scr!pt>}


    real	0m0.753s
    user	0m0.457s
    sys	0m0.048s


And in python3, a code is as follows.

    data = open( 'flag.txt' ).read()
    flag = []

    word = '<SCRIPT>'
    wordlen = len( word )

    for c in data:
        flag += [ c ]
        if c == '>' and ''.join( flag[-wordlen:] ).upper() == word:
            del( flag[-wordlen:] )

    print( ''.join( flag ) )

   

    time python3 rmscript.py 
    SECCON{sanitizing_is_not_so_good><_escaping_is_better_iPt><SCript<ScrIpT<scRIp<scRI<Sc<scr!pt>}


    real	0m8.846s
    user	0m8.761s
    sys	0m0.084s

I use a list named "flag" instead of strings in python3 program because of running time is very long by coping strings repeatedly if you use strings.
However, we can see a program made by C language is very faster than a program made by python3.

It is all of my solution for this problem.
