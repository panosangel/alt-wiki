# CD Ripping

## Install Software

```shell
sudo apt install abcde cd-discid cdparanoia eyed3 id3v2 lame flac
```

## Add configuration

Use the following configuration (`~/.abcde.conf`): 

```
# -----------------$HOME/.abcde.conf----------------- #
#
# A sample configuration file to convert music cds to
# FLAC
# using abcde version 2.9.3 release version.
#
# https://www.andrews-corner.org/abcde/
# minor mods by Panos (2025.01.12)
# -------------------------------------------------- #

# Encode tracks immediately after reading. Saves disk space, gives
# better reading of 'scratchy' disks and better troubleshooting of
# encoding process but slows the operation of abcde quite a bit:
LOWDISK=y

# Specify the method to use to retrieve the track information,
# the alternative is to specify 'musicbrainz':
CDDBMETHOD=cddb

# With the demise of freedb (thanks for the years of service!)
# we move to an alternative:
CDDBURL="https://gnudb.gnudb.org/~cddb/cddb.cgi"

# Specify the encoder to use for FLAC. In this case flac is the only choice.
FLACENCODERSYNTAX=flac

# Specify the path to the selected encoder.
FLAC=flac

# Options for FLAC
FLACOPTS='--verify --best'

# Output type for FLAC.
OUTPUTTYPE="flac"

# The cd ripping program to use.
CDROMREADERSYNTAX=cdparanoia

# Give the location of the ripping program and pass any extra options.
CDPARANOIA=cdparanoia  
CDPARANOIAOPTS="--never-skip=40"

EYED3OPTS="--non-std-genres --set-encoding=utf16-LE"

# Give the location of the CD identification program:       
CDDISCID=cd-discid 

# Give the base location here for the encoded music files.
OUTPUTDIR="$HOME/abcde"

# The default actions that abcde will take.
ACTIONS=cddb,playlist,read,encode,tag,move,clean

OUTPUTFORMAT='${ARTISTFILE} - ${ALBUMFILE} (${YEAR}) [${OUTPUT}]/${TRACKNUM}. ${TRACKFILE}'
VAOUTPUTFORMAT='Various - ${ALBUMFILE}/${TRACKNUM}. ${ARTISTFILE} - ${TRACKFILE}'

# This function takes out dots preceding the album name, and removes a grab
# bag of illegal characters. It allows spaces, if you do not wish spaces add
# in -e 's/ /_/g' after the first sed command.
mungefilename ()
{
  echo "$@" | sed -e 's/^\.*//' | tr -d ":><|*/\"'?[:cntrl:]"
}

# What extra options?
MAXPROCS=2                              # Run a few encoders simultaneously
PADTRACKS=y                             # Makes tracks 01 02 not 1 2
EXTRAVERBOSE=2                          # Useful for debugging
COMMENT='abcde version 2.9.3'           # Place a comment...
EJECTCD=y                               # Please eject cd when finished :-)
```

## Run
`abcde -V`

## Appendix A
- [abcde man page](https://www.mankier.com/1/abcde)  
- [abcde - A Better CD Encoder](https://abcde.einval.com/wiki/)  
- [How to Easily Rip a CD with Abcde](https://www.maketecheasier.com/rip-cd-with-abcde/)  
- [Automatically ripping CDs on linux](https://www.imagetracking.org.uk/2018/08/automatically-ripping-cds-on-linux/)  
- [Use abcde to produce high quality flac and mp3 output with album art under Xenial?](https://askubuntu.com/questions/788327/use-abcde-to-produce-high-quality-flac-and-mp3-output-with-album-art-under-xenia)  
- [tags not written #3](https://github.com/johnlane/abcde/issues/3)  
