---
layout: documentation
title: Multimedia
---

{% include base.html %}

# Multimedia

## Audio

openHAB is able to play sound either from the file system (files need to be put in the folder `conf/sounds`), from urls (e.g. Internet radio streams) or generated by text-to-speech engines (which are available as optional [Voice add-ons]({{base}}/addons/voice.html)).

There are different options for output devices (so called audio sinks): 

The distribution comes with these options built-in:

| `javasound` | System Speaker | This uses the JRE sound drivers to play audio to the local sound interface.
| `enhancedjavasound` | System Speaker (with mp3 support) | This uses the JRE sound drivers plus an additional 3rd party library, which adds support for mp3 files.
| `webaudio` | Web Audio | Convenient, if sounds should not be played on the server, but on the client: This sink sends the audio stream through HTTP to web clients, which then cause it to be played back by the browser. Obviously, the browser needs to be opened and have a compatible openHAB UI running. Currently, this feature is supported by Paper UI and HABPanel.

Additionally, certain bindings register their supported devices as audio sinks, e.g. Sonos speakers.

To check, which audio sinks are available, you can use the console:

```
openhab> smarthome:audio sinks
enhancedjavasound
javasound
webaudio
```

You can define the default audio sink either by textual configuration in `conf/services/runtime.cfg` or in the Paper UI in `Configuration->System->Audio`.

In order to play a sound, you can use

```
openhab> smarthome:audio play doorbell.mp3
```

on the console or

```
playSound("doorbell.mp3")
```

within DSL rules.

## Voice

### Text-to-Speech

In order to use text-to-speech, you need to install at least one [TTS service]({{base}}/addons/voice.html).
Once you have done so, you will find voices available in your system:

```
openhab> smarthome:voice voices
mactts:Jorge Jorge (es_ES)
mactts:Moira Moira (en_IE)
mactts:Alice Alice (it_IT)
mactts:Ioana Ioana (ro_RO)
mactts:Kanya Kanya (th_TH)
```

You can define a default TTS service and a default voice to use either by textual configuration in `conf/services/runtime.cfg` or in the Paper UI in `Configuration->System->Voice`.

In order to say a text, you can enter

```
openhab> smarthome:voice say Hello world!
```

on the console or

```
say("Hello world!")
```

within DSL rules.

### Speech-to-Text

Although there are already interfaces defined in openHAB for speech-to-text, up to now there is no add-on available for this functionality.
So the only choice that is available right now is to use the Android voice recognition feature that is built into the openHAB Android app. Currently, the Android app is [not yet adapted to the openHAB 2 voice features](https://github.com/openhab/openhab.android/issues/242), though - to use it, you will need to have a String item with the name "VoiceCommand" defined, to which the app sends any recognized text as a string command.

### Human Language Interpreter

Human language interpreters are meant to process prose that e.g. is a result of voice recognition or from other sources.

There are two implementations available by default:

| `rulehli` | Rule-based Interpreter | This mimicks the behavior of the Android app - it sends the string as a command to a (configurable, default is "VoiceCommand") item and expects a rule to pick it up and further process it.
| `system` | Built-in Interpreter | This is a simple implementation that understands basic home automation commands like "turn on the light" or "stop the music". It currently supports only English and German and the vocabulary is still very limited. The exact syntax still needs to be documented, for the moment you need to refer to the [source code](https://github.com/eclipse/smarthome/blob/master/bundles/core/org.eclipse.smarthome.core.voice/src/main/java/org/eclipse/smarthome/core/voice/internal/text/StandardInterpreter.java#L37).

To test the interpreter, you can use the command line (assuming you have an item called 'light'):

```
openhab> smarthome:voice interpret turn on the light
```

