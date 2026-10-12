# App usage guide

## Wiring

Turn off strip power before changing connections.

| Connection | Destination |
| --- | --- |
| Strip +5V | External 5V supply positive |
| Strip GND | Supply negative and ESP32 GND |
| Strip DIN | ESP32 GPIO13 / D13 |
| ESP32 USB | PC USB data port |

Use the strip's input end marked DIN. Do not power the strip through ESP32 VIN or PC USB. Choose a supply suitable for the strip's current requirements. Keep the data wire short; use a suitable 3.3V-to-5V data level shifter if signal reliability requires it.

Fold corners gently, away from LED packages. Do not sharply crease copper traces. Folded LEDs can remain connected and be configured to stay dark.

## Launch and connect

Open `release/app/ESP32 Sync Lights.exe`. On another PC, extract `lights.zip` to a short writable path and open `app/ESP32 Sync Lights.exe`. Keep the whole app folder together.

In **Settings → Mode**, select the screen and ESP32 COM port, then save. Find the current port in **Device Manager → Ports (COM & LPT)**. COM numbers can change between PCs and USB sockets.

Close other serial monitors or lighting apps before connecting. After changing a port, save again; reconnection starts without an app restart. Enable output explicitly after reconnecting.

## Configure the strip

Open **Settings → LEDs config**. Count physical positions from **LED 1 at DIN**, following the strip.

- **Top, right, bottom, left:** usable LEDs on each edge, excluding corner gaps.
- **Corner gaps:** LEDs at each of the four folds that must remain dark.
- **Start corner and direction:** match the installation using the numbered front-view preview.
- **Advanced offset and skipped ranges:** adjust the starting position or darken additional positions. Skips do not renumber later LEDs.

The physical total is all side counts plus all corner gaps. It must match the connected strip and be at most 300. Electrically connected unwanted LEDs still count.

Example: 72 top + 25 right + 72 bottom + 25 left, plus three one-LED corner gaps, gives **197 positions**. Enter your own measured counts.

Click **Save layout & settings**. The saved layout controls both screen sampling and the board's dark-LED mask.

## Test the layout

**Test one LED:** selects a physical position and lights it briefly at low brightness. It stops automatically; **Stop test** ends it early. Check numbering and direction this way.

**Screen color test:** opens the calibration pattern on the selected screen using the saved layout. Save first, select Bias light, and enable LEDs to compare screen edges and strip colors. The pattern does not enable output automatically.

Edit geometry in LEDs config. Screen-test geometry is read-only for the portable layout. Closing the test returns to settings.

## Brightness, screen sync, and effects

Select **Bias light** for screen sync. Use the brightness control to set output intensity; start low and increase after checking power and wiring.

Choose the available color or effect modes for ambient lighting without screen sync. If colors look washed out, check the selected monitor and sampling depth before changing calibration. A large sampled area blends colors; sampling a black border can make output dark.

For letterboxed videos, check sampling with ordinary desktop content and the screen test. Protected services such as Netflix may block capture. Turning off Chrome graphics acceleration is not required for normal use.

## Troubleshooting

| Problem | Check |
| --- | --- |
| No COM port | Data cable, USB socket, board USB serial driver |
| Connection fails | Current COM port, saved settings, other serial apps closed |
| LEDs dark | Output enabled, external power, common ground, correct DIN, matching count, saved layout |
| Flickering or wrong colors | Ground, power capacity, data wire and connectors, level shifting |
| Colors at wrong positions | Start, direction, offset, counts, gaps |
| Desktop works but video is dark | Black borders or protected capture |
| Monitor blacks out during capture | Stop capture, try another available capture backend, check graphics drivers |

Back up the app's `config/` folder before replacing your installed copy if you want to keep its layout. Share the clean ZIP rather than your configured app folder.
