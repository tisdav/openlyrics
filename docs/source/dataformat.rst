:tocdepth: 2

.. _dataformat:

.. highlight:: xml

Data Format
===========

Basic Structure
---------------

.. image:: /images/xmlstructure.png


Features
--------

.. glossary::

    categories
        ``<theme>``

    CCLI support
        ``<ccliNo>``

    chords
        ``<chord name="D">``

    comments in lyrics
        ``<verse><lines><comment/></lines></verse>``

    date of song release
        ``<releaseDate>``

    format version
        ``<song version="0.6>``

    keywords for searching
        ``<keywords>``

    last modification time
        ``<song modifiedDate="">``

    lines of text
        ``<line>``

    multiple authors
        ``<authors>``

    multiple categories
        ``<themes>``
        
    multiple song titles
        ``<titles>``

    multiple user-defined items
        ``<comments>``

    music properties
        ``<transposition>``
        ``<tempo>``
        ``<key>``

    namespace
        ``<song xmlns="http://openlyrics.info/namespace/2009/song">``

    parts
        ``<lines part="men">``

    slides
        ``<verse>``

    song book
        ``<collection>``
        ``<trackNo>``

    song metadata
        ``<song version="">``
        ``<song createdIn="">``
        ``<song modifiedIn="">``
        ``<song modifiedDate="">``

    song translator
        ``<author type="translator" lang="cs">``

    song variant
        ``<variant>``

    song version
        ``<customVersion>``

    tagging verse type
        ``<verse name="v1">``

    translated lyrics
        ``<verse name="v1" xml:lang="en">``

    translated song title
        ``<title xml:lang="en">``

    transposition
        ``<transposition>``

    user-defined item
        ``<comment>``

    verse order
        ``<verseOrder>``


Required Data Items
-------------------

The song, containing only necessary data items, follows::

    <song xmlns="http://openlyrics.info/namespace/2009/song"
          version="0.6"
          createdIn="OpenLP 1.9.0"
          modifiedIn="ChangingSong 0.0.1"
          modifiedDate="2010-01-28T13:15:30+01:00">
      <properties>
        <titles>
          <title>Amazing Grace</title>
        </titles>
      </properties>
      <lyrics>
        <verse name="v1">
          <lines>
            <line>Amazing grace how sweet the sound</line>
          </lines>
        </verse>
      </lyrics>
    </song>

As you can see from the previous example, a minimalistic song should contain
only:

* metadata
* title
* verse with one line of text

**Elements with empty values aren't allowed. If a data item is not present
in the song, the tag, where the data would be put, should not be in xml.**


Metadata
--------

Metadata are **required** to be present in every song. They should ease debugging
of of applications using OpenLyrics.

Metadata are enclosed in tag ``<song>`` as its attributes::

    <song xmlns="http://openlyrics.info/namespace/2009/song"
          version="0.6"
          createdIn="OpenLP 1.9.0"
          modifiedIn="ChangingSong 0.0.1"
          modifiedDate="2010-01-28T13:15:30+01:00">

xmlns
    Defines a xml namespace. The value should be always
    ``http://openlyrics.info/namespace/2009/song``

version
    Version of the OpenLyrics format used by a song. This allows applications
    to notify users, if the application doesn't support newer versions of
    OpenLyrics.

createdIn
    String to identify the application where a song was created for the first
    time. This
    attribute should be set when a new song is created. It should not be
    changed with additional updates and modification to the song. Even when
    the song is edited in another application. Recommended content of this
    attribute is *application name* and *version* like ``OpenLP 1.9.0``.

modifiedIn
    String to identify the application where a song was edited for the last time.
    This attribute should be set with every modification. Recommended content
    of this attribute is *application name* and *version* like ``OpenLP 1.9.0``.

modifiedDate
    Date and time of last modification. This attribute should be set with every
    modification. The used format of date is `ISO 8601
    <http://en.wikipedia.org/wiki/ISO_8601>`_. It should be in the format
    ``YYYY-MM-DDThh:mm:ss±[hh]:[mm]``.


Encoding and Filenames
----------------------

Encoding
^^^^^^^^

I recommend using `UTF-8 <http://en.wikipedia.org/wiki/Utf8>`_ encoding for the
content of xml files in OpenLyrics format. *UTF-8* is well supported among
programming libraries.

Filenames
^^^^^^^^^

In regards to filenames, the recommendation is to use such a name which will
well identify the song just by looking at the filename. For the file could be
used a combination of fields ``<titles>``, ``<variant>`` and/or ``<authors>``.
Since OpenLyrics is a xml based format, filenames should contain the extension
``.xml``

Examples::

    Amazing Grace.xml
    Amazing Grace (old hymn).xml
    Amazing Grace (John Newton).xml

It would be nice, if songs containing non ASCII characters in its title, use
also nos ASCII characters in filenames. These days all major operating systems
should support localized characters in filenames. However, there are some
limitation in this approach. Not all archive formats handle localized filenames
well. For example, one of most used archive formats, `ZIP
<http://en.wikipedia.org/wiki/ZIP_(file_format)>`_. On the other hand, the format
`7-Zip <http://en.wikipedia.org/wiki/7zip>`_ handles it well.


Song Properties
---------------

Description of all possible elements enclosed in tag ``<properties>``. Elements
enclosed in this tag may be any arbitrary order. For example it  doesn't matter
if tag ``<titles>`` occurs before ``<authors>``::
    
    <titles><title>Amazing Grace</title></titles>
    <authors><author>John Newton</author></authors>

Or ``<titles>`` occurs after ``<authors>``::
    
    <authors><author>John Newton</author></authors>
    <titles><title>Amazing Grace</title></titles>

**An application implementing OpenLyrics should not depend on any order of
elements enclosed in the tag ``<properties>``.**


Titles
^^^^^^

Title is a **mandatory element**. Every song must contain at least one title::

    <titles><title>Amazing Grace</title></titles>

There could be more titles::

    <titles>
      <title>Amazing Grace</title>
      <title>Amazing</title>
    </titles>

You can define attribute ``xml:lang=""``. It says what is the language of the
title. The value of this attribute should be in the format ``xx`` or ``xx-YY``
where ``xx`` is an `ISO-639 language code <http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes>`_
and YY is an `countro code <http://en.wikipedia.org/wiki/ISO_3166-1>`_. For
more details see `bcp47 <ftp://ftp.rfc-editor.org/in-notes/bcp/bcp47.txt>`_.

It comes handy when the song is translated from one
language to another and there is a need to know the titles in other languages
or the song contains lyrics in multiple languages::

    <titles>
      <title xml:lang="en">Amazing Grace</title>
      <title xml:lang="de">Staunenswerte Gnade</title>
      <title xml:lang="pl">Cudowna Boża łaska</title>
    </titles>

Addtitionally, it is possible use attribute ``original="true"``. This attribute
expresses that a title is the title of the original song::

    <titles>
      <title xml:lang="en" original="true">Amazing Grace</title>
      <title xml:lang="pl">Cudowna Boża łaska</title>
    </titles>


Authors
^^^^^^^

Authors is an optional element. When this element is present in the song,
there should be at least one subelement ``<author>``::

    <authors><author>John Newton</author></authors>

There could be more authors::

    <authors>
      <author>John Newton</author>
      <author>Johannes Newton</author>
    </authors>

Three types of authors can be distinguished:

* *author of words*::

      <author type="words">John Newton</author>

* *author of music*::

      <author type="music">John Newton</author>

* *translator*::

      <author type="translation" lang="cs">Jan Ňůtn</author>
  
  The translator type must in addition contain the attribute ``lang=""``.
  Value of this attribute should be in the format ``xx`` or ``xx-YY``.
  ``xx`` means language code and ``YY`` means country code.


Copyright
^^^^^^^^^

This element should contain copyright information. In some contries it is
necessary to display copyright information during presentation. In this
situation this comes handy.

This element could look like::

    <copyright>public domain</copyright>

Or for example::

    <copyright>1998 Vineyard Songs</copyright>


CCLI Number
^^^^^^^^^^^

`CCLI <http://www.ccli.com/>`_
stands for *Christian Copyright Licensing International*. CCLI offers
copyright licensing of songs and other resource materials for use in Christian
worship. For registered churches CCLI offers songs and other resources
for download. At CCLI an ID is assigned to every song. This element
provides integration with CCLI.

CCLI number (id) must be a positive integer::

    <ccliNo>22025</ccliNo>


Release Date
^^^^^^^^^^^^

Release date is a date/time when the song was released or published.

It can be just year::

    <releaseDate>1779</releaseDate>

year-month::

    <releaseDate>1779-09</releaseDate>

year-month-day::

    <releaseDate>1779-12-30</releaseDate>

year-month-day and time::

    <releaseDate>1779-12-31T13:15</releaseDate>


Transposition
^^^^^^^^^^^^^

This element is used when there is a need to move the key or the pitch of
chords up or down. The value must be a positive or negative integer.

The negative integer moves the pitch down by a fixed number of semitones::

    <transposition>-3</transposition>

The positive integer moves the pitch up by a fixed number of semitones::

    <transposition>4</transposition>


Tempo
^^^^^

Tempo means how the song should be played. It could be expressed in beats
per minute (bpm) or as any text value.

Tempo in bpm must be a positive integer in the range 30-250::

    <tempo type="bpm">90</tempo>

Tempo expressed as text can contain any arbitrary text. For example
``Very Fast``, ``Fast``, ``Moderate``, ``Slow``, ``Very Slow``, etc.::

    <tempo type="text">Moderate</tempo>


Key
^^^

Key determines the musical scale of a song. The value could be ``A``, ``B``,
``C#``, ``D``, ``Eb``, ``F#``, ``Ab``, etc.

Example::

    <key>Eb</key>


Variant
^^^^^^^

Variant should be used in situation, where there are 2 songs with the same
title and it is needed to distinguish those songs.

For example there could be two songs with the title *Amazing grace*. One song
published many years ago and one song published by a well known band, say
for instance *Newsboys*.

For the old song it could be::

    <variant>old hymn</variant>

For the song from a well known band::

    <variant>Newsboys</variant>


Publisher
^^^^^^^^^

Name of the publisher of the song::

    <publisher>Sparrow Records</publisher>


Custom Version
^^^^^^^^^^^^^^

Many songs aren't written at once. When a new song is written, it usually
contains just a title and lyrics. In the future the song will be updated
and more data will be added. This could help users distinguish different
versions of the same song.

This element can contain any arbitrary text which could help the user to
distinguish various song versions.

It could contain for example a version number::

    <customVersion>0.99</customVersion>

or date::

    <customVersion>2010-02-04</customVersion>

or anything else::

    <customVersion>this is previous version</customVersion>


Keywords
^^^^^^^^

Keywords are used to get more precise results when searching for a song in the
song database.

For the song *Amazing Grace* it could be::

    <keywords>amazing grace, how sweet the sound, a wretch like me</keywords>


Verse Order
^^^^^^^^^^^

Determines the order of verses in the lyrics. In lyrics part every verse is
enclosed in tag ``<verse>``. Every verse should have a different one word name.

Every word in ``<verseOrder>`` refers to name of verse in the ``<lyrics>``.
Verse name can appear in ``<verseOrder>`` multiple times.

Verse names should be in lowercase.

For example when in the lyrics part are verses with names *v1*, *v2*, and *c*,
this element could be::

    <verseOrder>v1 c v2 c v1 c</verseOrder>


Collection
^^^^^^^^^^

If a song comes from any collection or songbook, here should be noted the name
of the songbook or collection. There could be any arbitrary text::

    <collection>Name of a songbook or collection</collection>


Track Number
^^^^^^^^^^^^

If a song comes from any collection or songbook, here should be noted the
number of the song in a collection or songbook.
The value must be any positive integer::

    <trackNo>48</trackNo>


Themes
^^^^^^

Themes are used to categorize songs. Having songs categorized can be useful
when choosing songs for a ceremony or for a particular topic.

There can be one or more themes::

    <themes><theme>Adoration</theme></themes>

As additional attributes could be an ``id=""`` and/or ``xml:lang=""``::

    <themes>
      <theme>Adoration</theme>
      <theme id="1" xml:lang="en-US">Grace</theme>
      <theme id="2" xml:lang="en-US">Praise</theme> 
      <theme id="3" xml:lang="en-US">Salvation</theme>
      <theme id="1" xml:lang="pt-BR">Graça</theme>
      <theme id="2" xml:lang="pt-BR">Adoração</theme>
      <theme id="3" xml:lang="pt-BR">Salvação</theme>
    </themes>

The ``id`` attribute should be used when using a theme from a
`standardized CCLI list <http://www.ccli.com.au/owners/themes.cfm>`_.
The list can be found in the downloadable OpenLyrics archive in
file with name ``themelist.txt``. The value of ``id`` is the line number
of a particular theme in this file. Standardized themes with id should ease
assigning translated themes to songs in an application.

``xml:lang`` defines a language of a theme.
Value of this attribute should be in the format ``xx`` or ``xx-YY``.
``xx`` means language code and ``YY`` means country code.


Comments
^^^^^^^^

This field is for other additional unspecified user data. There can be more
items. The value can be any text::

    <comments>
      <comment>One of the most popular songs in our congregation.</comment>
      <comment>We sing this song often.</comment>
    </comments>



Song lyrics
-----------


Chords
------


Advanced Example
----------------

Here's an example of the XML::

    <?xml version="1.0" encoding="UTF-8"?>
    <song xmlns="http://openlyrics.info/namespace/2009/song"
          version="0.6"
          createdIn="OpenLP 1.9.0"
          modifiedIn="ChangingSong 0.0.1"
          <!-- date format: ISO 8601 -->
          modifiedDate="2009-12-22T21:24:30+02:00">
      <properties>
        <titles>
          <title>Amazing Grace</title>
        </titles>
        <authors>
          <author>John Newton</author>
        </authors>
        <copyright>Public Domain</copyright>
        <ccliNo>2762836</ccliNo>
        <releaseDate>1779</releaseDate>
        <tempo type="text">moderate</tempo>
        <key>D</key>
        <verseOrder>v1 v2 v3 v4 v5 v6</verseOrder>
        <themes>
          <theme>Assurance</theme>
          <theme>Grace</theme>
          <theme>Praise</theme>
          <theme>Salvation</theme>
        </themes>
      </properties>
      <lyrics>
        <verse name="v1">
          <lines>
            <line>Amazing grace how sweet the sound</line>
            <line>That saved a wretch like me.</line>
            <line>I once was lost, but now am found,</line>
            <line>Was blind but now I see.</line>
          </lines>
        </verse>
        <verse name="v2">
          <lines>
            <line>T'was grace that taught my heart to fear,</line>
            <line>And grace my fears;</line>
            <line>How precious did that grace appear</line>
            <line>The hour I first believed.</line>
          </lines>
        </verse>
        <verse name="v3">
          <lines>
            <line>Through many dangers, toil and snares,</line>
            <line>I have already come;</line>
            <line>'Tis grace has brought me safe thus far,</line>
            <line>And grace will lead me home.</line>
          </lines>
        </verse>
        <verse name="v4">
          <lines>
            <line>When we've been there ten thousand years</line>
            <line>Bright shining as the sun,</line>
            <line>We've no less days to sing God's praise</line>
            <line>Than when we've first begun.</line>
          </lines>
        </verse>
      </lyrics>
    </song>
