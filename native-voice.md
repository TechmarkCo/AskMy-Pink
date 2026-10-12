# Native Voice Processing Strategy for AskMy.pink

**Goal:** Offload speech-to-text (input) and text-to-speech (replies) to the phone’s native OS capabilities on Android and iOS. This keeps voice data on-device as long as possible, reduces latency, lowers costs, improves privacy, and works offline for the voice layer.

## Why Native?
Modern Android and iOS already ship high-quality on-device speech recognition and synthesis. By using them we:
- Avoid sending raw audio to our servers or third-party cloud STT/TTS
- Get lower latency (especially for replies)
- Reduce infrastructure cost (no paid speech APIs for every interaction)
- Support offline or low-connectivity use for the voice parts
- Feel more native and responsive to users

The AI research, recommendation, and habit-learning layers still run on our backend once we have clean text.

## Android
### Speech-to-Text (Input)
- Primary: `android.speech.SpeechRecognizer`
  - Use `createOnDeviceSpeechRecognizer()` (API 31+) when available for fully on-device recognition
  - Fall back to the default recognizer
- Alternative / enhanced: ML Kit GenAI Speech Recognition API (Basic or Advanced modes) for broader device support and better quality on newer Pixels
- Permissions: `RECORD_AUDIO`
- Best practice: Trigger offline model download for the user’s locale if needed

### Text-to-Speech (Replies)
- Use the system `TextToSpeech` engine
- Prefer the Google Speech Services / on-device voices when available
- Allow the user to choose system voices in settings

### Implementation notes
- Flutter: `speech_to_text` + `flutter_tts` or `stts` package (offline-first, native backends)
- React Native: wrappers around native modules or libraries like `react-native-nitro-voice` for fully offline paths
- Always prefer on-device flags when the device supports them

## iOS
### Speech-to-Text (Input)
- Primary: `SFSpeechRecognizer` with `requiresOnDeviceRecognition = true` (available since iOS 13, improved over time)
- iOS 26+: Prefer the new `SpeechAnalyzer` / `SpeechTranscriber` for better long-form and real-time on-device performance (no 1-minute session cap)
- Permissions: `NSSpeechRecognitionUsageDescription` + `NSMicrophoneUsageDescription` in Info.plist
- Check `supportsOnDeviceRecognition` for the selected locale before relying on it

### Text-to-Speech (Replies)
- Use `AVSpeechSynthesizer` + `AVSpeechUtterance`
- Select high-quality system voices (`AVSpeechSynthesisVoice`)
- Personal Voice (iOS 17+) can be used for accessibility-style personalization where appropriate

### Implementation notes
- Same cross-platform options as Android (Flutter/React Native plugins that call the native frameworks)
- On iOS 26+ consider SpeechAnalyzer for higher quality input

## Recommended Architecture for AskMy.pink
1. **User speaks** (voice button or always-on shortcut) → native STT on device produces text (e.g. "@wife where do you want to go eat around main street")
2. **App sends only the transcribed text** (plus minimal context like location) to the AskMy backend
3. **Backend AI** does research, habit-aware recommendations, booking suggestions, etc.
4. **Backend returns structured reply text** (original message + recommendations)
5. **App uses native TTS** to speak the reply (or key parts) to the user, and also shows it as a rich chat/notification
6. **Tagged person** receives the enriched message via push + in-app chat; they can also use native voice to reply if they want

## Benefits for the Product
- Feels as natural as talking to your spouse
- Voice never leaves the phone until it becomes text
- Works well on the go, even with spotty connectivity for the speech layer
- Lower operational cost
- Aligns with the mobile-first, easy-to-use vision

## Next Implementation Steps
- Choose primary stack (Flutter recommended for native access + single codebase)
- Prototype the voice capture → text → backend round-trip → native TTS flow
- Test on-device recognition quality for common family decision phrases
- Add user settings for voice preferences and language
- Document privacy (voice stays on device) in the app and on askmy.pink

This approach lets us take full advantage of the natural speech capabilities already built into Android and iOS while keeping the smart AI research and habit learning on our side.
