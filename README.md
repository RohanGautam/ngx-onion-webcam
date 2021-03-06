# ngx-onion-webcam

A simple Angular webcam component. Pure & minimal, no Flash-fallback.

<a href="https://rohangautam.github.io/ngx-onion-webcam/" target="_blank">See the Demo!</a>

**Plug-and-play.** This library contains a single module which can be imported into every standard Angular 9+ project.

**Simple to use.** The one component gives you full control and lets you take snapshots via actions and event bindings.

**Minimal.** No unnecessary Flash-fallbacks, no bundle-size bloating.

## Demo

Try out the <a href="https://rohangautam.github.io/ngx-onion-webcam/" target="_blank">Live-Demo</a>.

## Features

- Webcam live view
- Photo capturing
- Smartphone compatibility for modern OS's (OS must support [WebRTC/UserMedia API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices))
- Access to front- and back-camera, if multiple cameras exist
- Portrait & Landscape mode on smartphones
- Mirrored live-view for user-facing cameras - "selfie view" on phones
- Capturing of lossless pixel image data for better post-processing.

## Prerequisites

### Runtime Dependencies

**Note:** Starting from version `0.3.0` this project requires TypeScript `>= 3.7.0` (Angular 9). For older versions of Angular/TypeScript, please use version `0.2.6` of this library.

- Angular: `>=9.0.0`
- Typescript: `>=3.7.0`
- RxJs: `>=5.0.0`
- **Important:** Your app must be served on a secure context using `https://` or on localhost, for modern browsers to permit WebRTC/UserMedia access.

### Client

- [Current browser](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia#Browser_compatibility) w/ HTML5 and WebRTC/UserMedia support (Chrome >53, Safari >11, Firefox >38, Edge)
- Webcam / camera
- User permissions to access the camera

## Usage

This onion-skin version is not avaliable in npm. Clone the repo, and drop the `app/modules/webcam` folder into your project to be ready to use the `WebcamModule`.
In `app.module.ts`,

```typescript
import {WebcamModule} from './modules/webcam/webcam.module';
...
@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    WebcamModule
  ],
  providers: [],
  bootstrap: [...]
})
```

Now, you can use the `<webcam></webcam>` component in your pages.

## Options and Events

This section describes the basic inputs/outputs of the component. All inputs are optional.

### Inputs

- `trigger: Observable<void>`: An `Observable` to trigger image capturing. When it fires, an image will be captured and emitted (see Outputs).
- `width: number`: The maximal video width of the webcam live view.
- `height: number`: The maximal video height of the webcam live view. The actual view will be placed within these boundaries, respecting the aspect ratio of the video stream.
- `videoOptions: MediaTrackConstraints`: Defines constraints ([MediaTrackConstraints](https://developer.mozilla.org/en-US/docs/Web/API/MediaTrackConstraints)) to apply when requesting the video track.
- `mirrorImage: string | WebcamMirrorProperties`: Flag to control image mirroring. If the attribute is missing or `null` and the camera claims to be user-facing, the image will be mirrored (x-axis) to provide a better user experience ("selfie view"). A string value of `"never"` will prevent mirroring, whereas a value of `"always"` will mirror every camera stream, even if the camera cannot be detected as user-facing. For future extensions, the `WebcamMirrorProperties` object can also be used to set these values.
- `allowCameraSwitch: boolean`: Flag to enable/disable camera switch. If enabled, a switch icon will be displayed if multiple cameras are found.
- `switchCamera: Observable<boolean|string>`: Can be used to cycle through available cameras (true=forward, false=backwards), or to switch to a specific device by deviceId (string).
- `captureImageData: boolean = false`: Flag to enable/disable capturing of a lossless pixel ImageData object when a snapshot is taken. ImageData will be included in the emitted `WebcamImage` object.
- `imageType: string = 'image/jpeg'`: [Image type](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL) to use when capturing snapshots. Default is 'image/jpeg'.
- `imageQuality: number = 0.92`: [Image quality](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL) to use when capturing snapshots. Must be a number between 0..1. Default is 0.92.

### Outputs

- `imageCapture: EventEmitter<WebcamImage>`: Whenever an image is captured (i.e. triggered by `[trigger]`), the image is emitted via this `EventEmitter`. The image data is contained in the `WebcamImage` data structure as both, plain Base64 string and data-url.
- `imageClick: EventEmitter<void>`: An `EventEmitter` to signal clicks on the webcam area.
- `initError: EventEmitter<WebcamInitError>`: An `EventEmitter` to signal errors during the webcam initialization.
- `cameraSwitched: EventEmitter<string>`: Emits the active deviceId after the active video device has been switched.

## Good To Know

### How to determine if a user has denied camera access

When camera initialization fails for some reason, the component emits a `WebcamInitError` via the `initError` EventEmitter. If provided by the browser, this object contains a field `mediaStreamError: MediaStreamError` which contains information about why UserMedia initialization failed. According to [Mozilla API docs](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia), this object contains a `name` attribute which gives insight about the reason.

> If the user denies permission, or matching media is not available, then the promise is rejected with NotAllowedError or NotFoundError respectively.

Determine if a user has denied permissions:

```
  <webcam (initError)="handleInitError($event)"></webcam>
```

```
  public handleInitError(error: WebcamInitError): void {
    if (error.mediaStreamError && error.mediaStreamError.name === "NotAllowedError") {
      console.warn("Camera access was not allowed by user!");
    }
  }
```

### Publish

Run `npm publish` to publish to github packages.

## Development

Here you can find instructions on how to start developing this library.

### Build

Run `npm run packagr` to build the library. The build artifacts will be stored in the `dist/` directory.

### Start

Run `npm start` to build and run the surrounding demo app with the `WebcamModule`. Essential for live-developing.

### Generate docs/

Run `npm run docs` to generate the live-demo documentation pages in the `docs/` directory.

### Running Unit Tests

Run `npm run test` to run unit-tests.
