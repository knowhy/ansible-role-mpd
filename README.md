[![Build Status](https://travis-ci.org/knowhy/ansible-role-mpd.svg?branch=master)](https://travis-ci.org/knowhy/ansible-role-mpd)

ansible-role-mpd
================

Ansible role to install MPD - music player daemon.

Requirements
------------

None

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

	mpd_root_path: "~/.mpd"

The root path for all databases and files for MPD.

    mpd_music_directory: "~/music"

This setting controls the top directory which MPD will search to discover the available audio files and add them to the daemon's online database. This setting defaults to the XDG directory, otherwise the music directory will be disabled and audio files will only be accepted over ipc socket (using file:// protocol) or streaming files over an accepted protocol.

    mpd_playlist_directory: "{{ mpd_root_path }}/playlists"

This setting sets the MPD internal playlist directory. The purpose of this directory is storage for playlists created by MPD. The server will use playlist files not created by the server but only if they are in the MPD format. This setting defaults to playlist saving being disabled.

    mpd_db_file: "{{ mpd_root_path }}/database"

This setting sets the location of the MPD database. This file is used to load the database at server start up and store the database while the server is not up. This setting defaults to disabled which will allow MPD to accept files over ipc socket (using file:// protocol) or streaming files over an accepted protocol.

    mpd_log_file: "{{ mpd_root_path }}/log"
 
These settings are the locations for the daemon log files for the daemon. These logs are great for troubleshooting, depending on your log_level settings. The special value "syslog" makes MPD use the local syslog daemon. This setting defaults to logging to syslog, otherwise logging is disabled. 

	mpd_pid_file: "{{ mpd_root_path }}/pid"

This setting sets the location of the file which stores the process ID for use of mpd --kill and some init scripts. This setting is disabled by default and the pid file will not be stored.

	mpd_state_file:	"{{ mpd_root_path }}/state"

This setting sets the location of the file which contains information about most variables to get MPD back into the same general shape it was in before it was brought down. This setting is disabled by default and the server state will be reset on server start up.

	mpd_sticker_file: "{{ mpd_root_path }}/sticker.sql"

The location of the sticker database.  This is a database which manages dynamic information attached to songs.

	mpd_user: mpd

This setting specifies the user that MPD will run as. MPD should never run as root and you may use this setting to make MPD change its user ID after initialization. This setting is disabled by default and MPD is run as the current user.

	mpd_group: mpd

This setting specifies the group that MPD will run as. If not specified primary group of user specified with "user" setting will be used (if set). This is useful if MPD needs to be a member of group such as "audio" to have permission to use sound card.

	mpd_bind_address: any
	mpd_socket: "{{ mpd_root_path }}/socket"

This setting sets the address and socket for the daemon to listen on. Careful attention should be paid if this is assigned to anything other then the default, any. This setting can deny access to control of the daemon.

	mpd_port: 6600

This setting is the TCP port that is desired for the daemon to get assigned to.

	mpd_log_level: default

This setting controls the type of information which is logged. Available setting arguments are "default", "secure" or "verbose". The "verbose" setting argument is recommended for troubleshooting, though can quickly stretch available resources on limited hardware storage.

	mpd_gapless_mp3_playback: yes

If you have a problem with your MP3s ending abruptly it is recommended that you set this argument to "no" to attempt to fix the problem. If this solves the problem, it is highly recommended to fix the MP3 files with vbrfix (available from <http://www.willwap.co.uk/Programs/vbrfix.php>), at which point gapless MP3 playback can be enabled.

	mpd_restore_paused: no

Setting "restore_paused" to "yes" puts MPD into pause mode instead of starting playback after startup.

	mpd_save_absolute_paths_in_playlists: no

This setting enables MPD to create playlists in a format usable by other music players.

	mpd_metadata_to_use: "artist,album,title,track,name,genre,date,composer,performer,disc"

This setting defines a list of tag types that will be extracted during the audio file discovery process. The complete list of possible values can be found in the user manual.

	mpd_auto_update: yes

This setting enables automatic update of MPD's database when files in music_directory are changed.

	mpd_auto_update_depth: 3

Limit the depth of the directories being watched, 0 means only watch the music directory itself.  There is no limit by default.

	mpd_follow_outside_symlinks: yes

If this setting is set to "yes", MPD will discover audio files by following symbolic links outside of the configured music_directory.

	mpd_follow_inside_symlinks: yes

If this setting is set to "yes", MPD will discover audio files by following symbolic links inside of the configured music_directory.

	mpd_zeroconf_enabled: yes

If this setting is set to "yes", service information will be published with Zeroconf / Avahi.

	mpd_zeroconf_name: "Music Player"

The argument to this setting will be the Zeroconf / Avahi unique name for this MPD server on the network.

	mpd_default_permissions: "read,add,control,admin"

This setting specifies the permissions a user has who has not yet logged in.

	mpd_authorization_data:
	- { password: password
	    permissions: "read,add,control,admin"
	  }

If this setting is set, MPD will require password authorization. The password can setting can be specified multiple times for different password profiles.

	mpd_database_proxy: False
	mpd_database_proxy_host: other.mpd.host
	mpd_database_proxy_port: 6600

If set to True MPD will use remote database.

	mpd_curl_proxy: False
	mpd_curl_proxy_host: proxy.isp.com:8080
	mpd_curl_proxy_user: user
	mpd_curl_proxy_password: password

If set to True mpd will use mpd_curl_proxy.

	mpd_audio_output_alsa_enabled: True
	mpd_audio_output_alsa_name: "My ALSA Device"
	mpd_audio_output_alsa_device: "hw:0,0" (optional)
	mpd_audo_output_alsa_mixer_type: hardware (optional)
	mpd_audio_output_alsa_mixer_device: default (optional)
	mpd_audio_output_alsa_mixer_control: PCM (optional)
	mpd_audio_output_alsa_mixer_index: 0 (optional)

If set to True mpd will use configured ALSA audio output.

	mpd_audio_output_oss_enabled: False
	mpd_audio_output_oss_name: "My OSS Device"
	mpd_audio_output_oss_device: /dev/dsp (optional)
	mpd_audio_output_oss_mixer_type: hardware (optional)
	mpd_audio_output_oss_mixer_device: "/dev/dsp" (optional)
	mpd_audio_output_oss_mixer_control: PCM (optional)

If set to True mpd will use configured OSS audio output.

	mpd_audio_output_shout_enabled: False
	mpd_audio_output_shout_encoder: vorbis (optional)
	mpd_audio_output_shout_name: "My Shout Stream"
	mpd_audio_output_shout_host: localhost
	mpd_audio_output_shout_port: 8000
	mpd_audio_output_shout_mount: "/mpd.ogg"
	mpd_audio_output_shout_password: hackme
	mpd_audio_output_shout_quality: 5.0
	mpd_audio_output_shout_bitrate: 128
	mpd_audio_output_shout_format: 44100:16:1
	mpd_audio_output_shout_protocol: icecast2 (optional)
	mpd_audio_output_shout_user: shout (optional)
	mpd_audio_output_shout_description: "My Stream Description" (optional)
	mpd_audio_output_shout_url: http://example.com (optional)
	mpd_audio_output_shout_genre: jazz (optional)
	mpd_audio_output_shout_public: no (optional)
	mpd_audio_output_shout_timeout: 2 (optional)
	mpd_audio_output_shout_mixer_type: software (optional)

If set to True MPD will be configured with Icecast shout output.

	mpd_audio_output_recorder_enabled: False
	mpd_audio_output_recorder_name: "My recorder"
	mpd_audio_output_recorder_encoder: vorbis (optional, vorbis or lame)
	mpd_audio_output_recorder_path: /var/lib/mpd/recorder/mpd.ogg
	mpd_audio_output_recorder_quality: 5.0 (do not define if bitrate is defined)
	mpd_audio_output_recorder_bitrate: 128 (do not define if quality is defined)
	mpd_audio_output_recorder_format: 44100:16:1

If set to True MPD will be configured with recoder output.

	mpd_audio_output_httpd_enabled: False
	mpd_audio_output_httpd_name: "My HTTP Stream"
	mpd_audio_output_httpd_encoder: vorbis (optional, vorbis or lame)
	mpd_audio_output_httpd_port: 8000
	mpd_audio_output_httpd_bind_to_address: 0.0.0.0 (optional, IPv4 or IPv6)
	mpd_audio_output_httpd_quality: 5.0 (do not define if bitrate is defined)
	mpd_audio_output_httpd_bitrate: 128 (do not define if quality is defined)
	mpd_audio_output_httpd_format: 44100:16:1
	mpd_audio_output_httpd_max_clients: 0 (optional, 0=no limit)

If set to True MPD will be configured with httpd output (built-in HTTP streaming server):

	mpd_audio_ouput_pulse_enabled: False
	mpd_audio_ouput_pulse_name: "My Pulse Output"
	mpd_audio_ouput_pulse_server: remove_server (optional)
	mpd_audio_ouput_pulse_sink: remote_server_sink (optional)

If set to True MPD will be configured with pulseaudio output (streaming to a remote pulseaudio server)

	mpd_audio_output_winmm_enabled: False
	mpd_audio_output_winmm_name: "My WinMM output"
	mpd_audio_output_winmm_device: "Digital Audio (S/PDIF) (High Definition Audio Device)" (or 0, optional)
	mpd_audio_output_winmm_mixer_type: hardware (optional)

If set to True MPD will be configured with winmm output (Windows multimedia API).

	mpd_audio_output_openal_enabled: False
	mpd_audio_output_openal_name: "My OpenAL output"
	mpd_audio_output_openal_device: "Digital Audio (S/PDIF) (High Definition Audio Device)" (optional)

If set to True MPD will be configured with openal output.

	mpd_audio_output_pipe_enabled: False
	mpd_audio_output_pipe_name: "my pipe"
	mpd_audio_output_pipe_command: "aplay -f cd 2>/dev/null"
	mpd_audio_output_pipe_format: "44100:16:2"

If set to True MPD will be configured with pipe output defined by mpd_audio_output_pipe_command variable. To use AudioCompress set mpd_audio_output_pipe_command to "AudioCompress -m | aplay -f cd 2>/dev/null", to send raw PCM stream through PCM set mpd_audio_output_pipe_command to "nc example.org 8765".

	mpd_audio_output_null_enabled: False
	mpd_audio_output_null_name: "My Null Output"
	mpd_audio_output_null_mixer_type: none (optional)

If set to True MPD will be configured with null output (for no audio output).

	mpd_samplerate_converter_enabled: False
	mpd_samplerate_converter: "Fastest Sinc Interpolator"

If MPD has been compiled with libsamplerate support, this setting specifies the sample rate converter to use.  Possible values can be found in the mpd.conf man page or the libsamplerate documentation. By default, this is setting is disabled.

	mpd_replaygain: off

Normalization automatic volume adjustments: This setting specifies the type of ReplayGain to use. This setting can have the argument "off", "album", "track" or "auto". "auto" is a special mode that chooses between "track" and "album" depending on the current state of random playback. If random playback is enabled then "track" mode is used. See <http://www.replaygain.org> for more details about ReplayGain. This setting is off by default.

	mpd_replaygain_preamp: 0

This setting sets the pre-amp used for files that have ReplayGain tags. By default this setting is disabled.

	mpd_replaygain_missing_preamp: 0

This setting sets the pre-amp used for files that do NOT have ReplayGain tags. By default this setting is disabled.

	mpd_replaygain_limit: yes

This setting enables or disables ReplayGain limiting. MPD calculates actual amplification based on the ReplayGain tags and replaygain_preamp / replaygain_missing_preamp setting. If replaygain_limit is enabled MPD will never amplify audio signal above its original level. If replaygain_limit is disabled such amplification might occur. By default this setting is enabled.

	mpd_volume_normalization: no

This setting enables on-the-fly normalization volume adjustment. This will result in the volume of all playing audio to be adjusted so the output has  equal "loudness". This setting is disabled by default.

	mpd_filesystem_charset: "UTF-8"

If file or directory names do not display correctly for your locale then you may need to modify this setting.

	mpd_id3v1_encoding: ISO-8859-1

This setting controls the encoding that ID3v1 tags should be converted from.

	sidplay_decoder_enabled: False
	sidplay_decoder_songlength_database: /media/C64Music/DOCUMENTS/Songlengths.txt
	sidplay_decoder_default_songlength: 120
	sidplay_decoder_filter: True


If set to True MPD will be configured with SIDPlay decoder, songlength_database: Location of your songlengths file, as distributed with the HVSC. The sidplay plugin checks this for matching MD5 fingerprints. See http://www.c64.org/HVSC/DOCUMENTS/Songlengths.faq
default_songlength: This is the default playing time in seconds for songs not in the songlength database, or in case you're not using a database.  A value of 0 means play indefinitely. filter: Turns the SID filter emulation on or off.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: knowhy.mpd }

License
-------

BSD

Author Information
------------------

This role was created in 2016 by Henrik Pingel.
