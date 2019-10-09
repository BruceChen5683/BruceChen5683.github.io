编译
```
android 编译流程
```

  apple m3u8

    What duration should media files be?
      A duration of 10 seconds of media per file seems to strike a reasonable balance for most broadcast content.
    What data rates are supported?
      The streaming protocol itself places no limitations on the data rates that can be used.
      The current implementation has been tested using audio-video streams with data rates as low as 64 Kbps and as high as 3 Mbps to iPhone.

  Preparing Media for Delivery to iOS-Based Devices
    https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/UsingHTTPLiveStreaming/UsingHTTPLiveStreaming.html#//apple_ref/doc/uid/TP40008332-CH102-SW8

  HLS(http live streaming )
    It allows a receiver to adapt the bit rate of the media to the current network conditions in order to maintain uninterrupted playback at the best possible quality.

    A more complex presentation can be described by a Master Playlist.
    A Master Playlist provides a set of Variant Streams, each of which describes a different version of the same content.

  MPEG-2 Transport Streams
    The first two Transport Stream packets in a Segment without an EXT-X-MAP tag SHOULD be a PAT and a PMT.

  音频视频同步
    In order to synchronize timestamps between audio/video and subtitles,a X-TIMESTAMP-MAP metadata header SHOULD be added to each WebVTT header.  This header maps WebVTT cue timestamps to MPEG-2 (PES) timestamps in other Renditions of the Variant Stream.  Its format is:
      X-TIMESTAMP-MAP=LOCAL:<cue time>,MPEGTS:<MPEG-2 time>
      e.g. X-TIMESTAMP-MAP=LOCAL:00:00:00.000,MPEGTS:900000

  Playlists ********************************************************************
      Each Playlist file MUST be identifiable by either the path component
      of its URI or by HTTP Content-Type.  In the first case, the path MUST
      end with either .m3u8 or .m3u.  In the second, the HTTP Content-type
      MUST be "application/vnd.apple.mpegurl" or "audio/mpegurl".  Clients
      SHOULD refuse to parse Playlists that are not so identified.

      Clients SHOULD reject Playlists which contain a BOM or do not parse as UTF-8.


      A Playlist is a Media Playlist if all URI lines in the Playlist identify Media Segments.  A Playlist is a Master Playlist if all URI lines in the Playlist identify Media Playlists.  A Playlist MUST be either a Media Playlist or a Master Playlist; all other Playlists are invalid.


          The segment bit rate of a Media Segment is the size of the Media
       Segment divided by its EXTINF duration (Section 4.3.2.1).  Note that
       this includes container overhead but does not include overhead
       imposed by the delivery system, such as HTTP, TCP or IP headers.

       The peak segment bit rate of a Media Playlist is the largest bit rate
       of any contiguous set of segments whose total duration is between 0.5
       and 1.5 times the target duration.  The bit rate of a set is
       calculated by dividing the sum of the segment sizes by the sum of the
       segment durations.

       The average segment bit rate of a Media Playlist is the sum of the
       sizes (in bits) of every Media Segment in the Media Playlist, divided
       by the Media Playlist duration.  Note that this includes container
       overhead, but not HTTP or other overhead imposed by the delivery
       system.


       Clients MUST reject Playlists that contain both Media Segment Tags and Master Playlist tags (Section 4.3.4).