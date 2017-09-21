## Goal

Games without music and sound effects is like a whole day without bread. It is well knows by everyone Tetris’ background music, while the blocks are falling; or the sound effect of Mario Bros. jumping, even taking a coin. Wave Engine makes it super easy to add those to your games, you will learn how to achieve such with just a few steps.

Note: Wave Engine also supports 3D sounds, which is not covered on this recipe.

## Hands-on

### With Wave Visual Editor

Start by importing both a sound file (WAV format, for example) and a music one (i.e., MP3). Enter on the Assets folder, at Asset Details panel, right-click button, Import Asset.

A double click on the MP3 file will launch OS by-default player. However, doing the same on the WAV one will open the Asset Viewer window, with a live preview of the sound, and some characteristics which can be tweaked:
* ZipCompress: whether the intermediate file generated is also zipped (default), reducing on-disk memory. The first time a compressed asset is loaded into a game, it is uncompressed for future access.
* ChannelFormat: Stereo (default) or Mono. Note: in order to add the sound to be used in 3D, you must change this value to mono.
* AudioQuality: Low, Medium or High (default).

The rest of steps are done in code, so please continue with below section.

### With Visual Studio/Xamarin Studio

Once the sound and music files are added through Wave Visual Editor, open the associated C# solution (remember: File > Open C# Solution…)

#### Playing Sound

There are two pieces which "play" together to reproduce sounds in Wave Engine: [SoundBank](xref:WaveEngine.Framework.Sound.SoundBank) and [SoundPlayer](xref:Wave​Engine.​Framework.​Services.SoundPlayer) service. `SoundBank` allows us to associate every sound to be played, with the option to define an [AssetsContainer](xref:Wave​Engine.​Framework.​Services.AssetsContainer) (this will assure, for instance, to be unloaded when an `Scene` ends, if used this last one's container). `SoundPlayer` is the service in charge of registering the previous bank, plus playing sounds contained within it.

```c#
var soundBank = new SoundBank(Assets);
// Sound.wav was added through Wave Visual Editor
var sound = new SoundInfo(WaveContent.Assets.Sound_wav);

soundBank.Add(sound);

WaveServices.SoundPlayer.RegisterSoundBank(soundBank);
WaveServices.SoundPlayer.Play(sound);
```

#### Playing Music

Music follows a similar pattern, without the need to work with a bank, just the [MusicPlayer](xref:Wave​Engine.​Framework.​Services.MusicPlayer).

```c#
// Song.mp3 was added through Wave Visual Editor
var music = new MusicInfo(WaveContent.Assets.Song_mp3);

WaveServices.MusicPlayer.Play(music);
```

Both sound and music player services contain multiple methods which allow common use cases like stopping, pausing, etc. Please refer to their [API](xref:WaveEngine) to see a full description of those.

## Wrap-up


We have learned how to play sound effects and songs. Adding those files to our project is done through Wave Visual Editor, while the playback is handled by code.