Audio File Convert.

Syntax
      afconvert [option...] input_file [output_file]

      afconvert input_file [-o output_file [option...]]...
Options may appear before or after the direct arguments. If output_file is not specified,
a name is generated programmatically and the file is written into the same directory as input_file.

Output file options apply to the previous output_file.
Other options may appear anywhere.

General options:
    { -d | --data } data_format[@sample_rate][/format_flags][#frames_per_packet]
        [-][BE|LE]{F|[U]I}{8|16|24|32|64}          (PCM)
            e.g.   BEI16   F32@44100
        or a data format appropriate to file format (see -hf)
        format_flags: hex digits, e.g. '80'
        Frames per packet can be specified for some encoders, e.g.: samr#12
        A format of "0" specifies the same format as the source file,
            with packets copied exactly.
        A format of "N" specifies the destination format should be the
            native format of the lossless encoded source file (alac, FLAC only)

    { -c | --channels } number_of_channels
        add/remove channels without regard to order.

    { -l | --channellayout } layout_tag
        layout_tag: name of a constant from CoreAudioTypes.h
          (prefix "kAudioChannelLayoutTag_" may be omitted)
        If specified once, applies to output file; if twice, the first
          applies to the input file, the second to the output file

    { -b | --bitrate } total_bit_rate_bps
         e.g. 256000 will give you roughly:
             for stereo source: 128000 bits per channel
             for 5.1 source: 51000 bits per channel
                 (the .1 channel consumes few bits and can be discounted in the
                 total bit rate calculation)

    { -q | --quality } codec_quality
        codec_quality: 0-127

    { -r | --src-quality } src_quality
        Src_quality (sample rate converter quality): 0-127 (default is 127)

    { --src-complexity } src_complexity
        src_complexity (sample rate converter complexity): line, norm, bats

    { -s | --strategy } strategy
        Bitrate allocation strategy for encoding an audio track
        0 for CBR, 1 for ABR, 2 for VBR_constrained, 3 for VBR

    --prime-method method
        decode priming method (see AudioConverter.h)

    --prime-override samples_prime samples_remain
        Can be used to override the priming information stored in the source
        file to the specified values. If -1 is specified for either, the value
        in the file is used.

    --no-filler
        Don’t page-align audio data in the output file

    --soundcheck-generate
        Analyse audio, add SoundCheck data to the output file

    --media-kind "media kind string"
        media kinds are: "Audio Ad", "Video Ad"

    --anchor-loudness
        Set a single precision floating point value to
        indicate the anchor loudness of the content in dB

    --generate-hash
        generate an SHA-1 hash of the input audio data and add it to the output file.

    --codec-manuf codec_manuf
        Specify the codec with the specified 4-character component manufacturer code

    --dither algorithm
        Algorithm: 1-2

    --mix
        enable channel downmixing

    { -u | --userproperty } property value
        Set an arbitrary AudioConverter property to a given value
        property is a four-character code; value can be a signed
        32-bit integer or a single precision floating point value.
        e.g. '-u vbrq sound_quality' sets the sound quality level
             (sound_quality: 0-127)
        May not be used in a transcoding situation.

    -ud property value
        Identical to -u except only applies to a decoder. Fails if there is no decoder.

    -ue property value
        Identical to -u except only applies to an encoder. Fails if there is no encoder.

Input file options:
    --read-track track_index
        For input files containing multiple tracks, the index (0..n-1)
        of the track to read and convert.

    --offset number_of_frames
        The starting offset in the input file in frames. (The first frame is
        frame zero.)

    --soundcheck-read
        Read SoundCheck data from source file and set it on any destination
        file(s) of appropriate filetype (.m4a, .caf).

    --copy-hash
        Copy an SHA-1 hash chunk, if present, from the source file to the output file.

    --gapless-before filename
        File coming before the current input file of a gapless album

    --gapless-after filename
        File coming after the current input file of a gapless album

Output file options:
    -o filename
        Specify an (additional) output file.
    { -f | --file } file_format
        use -hf for a complete list of supported file/data formats

    --condensed-framing field_size_in_bits
        Specify storage size in bits for externally framed packet sizes.
        Supported value is 16 for aac in m4a file format.

Other options:
    { -v | --verbose }
        Print progress verbosely

    { -t | --tag }
        If encoding to CAF, store the source file’s format and name in a user
        chunk. If decoding from CAF, use the destination format and filename
        found in a user chunk.

    { --leaks }
        Run leaks at the end of the conversion

    { --profile }
        Collect and print performance information

Help options:
    { -hf | --help-formats }
        Print a list of supported file/data formats, as below.

    { -h | --help }
        Print help
Audio file (-f) and the data formats (-d) which each supports:

    '3gpp' = 3GP Audio (.3gp)
               data_formats: 'Qclp' 'aac ' 'aace' 'aacf' 'aach' 'aacl'
                             'aacp' 'samr' 
    '3gp2' = 3GPP-2 Audio (.3g2)
               data_formats: 'Qclp' 'aac ' 'aace' 'aacf' 'aach' 'aacl'
                             'aacp' 'samr'
    'adts' = AAC ADTS (.aac, .adts)
               data_formats: 'aac ' 'aach' 'aacp'
    'ac-3' = AC3 (.ac3)
               data_formats: 'ac-3'

    'AIFC' = AIFC (.aifc, .aiff, .aif)
               data_formats: I8 BEI16 BEI24 BEI32 BEF32 BEF64 UI8 'ulaw'  'alaw'
                             'MAC3' 'MAC6' 'ima4' 'QDMC' 'QDM2'  'Qclp' 'agsm'

    'AIFF' = AIFF (.aiff, .aif)
               data_formats: I8 BEI16 BEI24 BEI32

    'amrf' = AMR (.amr)
               data_formats: 'samr' 'sawb'

    'm4af' = Apple MPEG-4 Audio - Lossless (.m4a, .m4r)
               data_formats: 'aac ' 'aace' 'aacf' 'aach' 'aacl' 'aacp'
                             'ac-3' 'alac' 'ec-3' 'paac' 'pac3' 'qec3' 'zec3'

    'm4bf' = Apple MPEG-4 AudioBooks (.m4b)
               data_formats: 'aac ' 'aace' 'aacf' 'aach' 'aacl' 'aacp' 'paac'

    'caff' = CAF - Apple Core Audio Format (.caf)
               data_formats: '.mp1' '.mp2' '.mp3' 'QDM2' 'QDMC' 'Qclp'
                             'Qclq' 'aac ' 'aace' 'aacf' 'aach' 'aacl'
                             'aacp' 'ac-3' 'alac' 'alaw' 'dvi8' 'ec-3'
                             'ilbc' 'ima4' I8 BEI16 BEI24 BEI32 BEF32
                             BEF64 LEI16 LEI24 LEI32 LEF32 LEF64 'ms\x00\x02'
                             'ms\x00\x11' 'ms\x001' 'ms \x00' 'paac' 'pac3'
                             'pec3' 'qaac' 'qac3' 'qach' 'qacp' 'qec3'
                             'samr' 'ulaw' 'zaac' 'zac3' 'zach' 'zacp'  'zec3'

    'ec-3' = EC3 (.ec3)
               data_formats: 'ec-3'
    'MPG1' = MPEG Layer 1 (.mp1, .mpeg, .mpa)
               data_formats: '.mp1'
    'MPG2' = MPEG Layer 2 (.mp2, .mpeg, .mpa)
               data_formats: '.mp2' 
    'MPG3' = MPEG Layer 3 (.mp3, .mpeg, .mpa)
               data_formats: '.mp3'

    'mp4f' = MPEG-4 Audio - Lossy (.mp4)
               data_formats: 'aac ' 'aace' 'aacf' 'aach' 'aacl' 'aacp'
                             'ac-3' 'ec-3' 'qec3' 'zec3'

    'NeXT' = NeXT/Sun (.snd, .au)
               data_formats: I8 BEI16 BEI24 BEI32 BEF32 BEF64 'ulaw'
    'Sd2f' = Sound Designer II (.sd2)
               data_formats: I8 BEI16 BEI24 BEI32
    'WAVE' = WAVE (.wav)
               data_formats: UI8 LEI16 LEI24 LEI32 LEF32 LEF64 'ulaw' 'alaw'
You can specify the data format (with -d) and the output file format (with -f) consult the table above to see which data formats are supported by the desired file format.

It is important to give the new data file an appropriate file extension, afconvert will happily read and write from/to any filename, but other programs are less forgiving. The normal file extensions are shown in brackets in the list above.

MacOS and afconvert do not include an MP3 encoder, though there is a MP4 (lossy) encoder. If you need to create MP3, use Lame or iTunes.

Examples

Convert an Audio MP3 File to an iPhone Ringtone (m4af file):

$ afconvert input.mp3 ringtone.m4r --file m4af

Convert an AAC audio file to the iPhone 'Core Audio File Format' (CAFF) at a low 32 kbs bitrate:

$ afconvert --data aac --bitrate 32000 input.aac output.caf --file 'caff'

Convert a WAV file to an MP4 file with lossy aac format at a high bitrate of 256 kbs:

$ afconvert -d aac -b 256000 input.wav output.mp4 -f mp4f


