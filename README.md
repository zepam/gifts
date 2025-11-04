# Random Gift Drawing   
<img src="gifts.jpg" alt="Gifts" width="220" style="float:right;margin-left:16px;" />

A small single-file web app for running a Secret-Santa gift-draw with exclusions. The repo contains a fun slot-machine-style UI that reveals who each person is assigned to give a gift to. It supports specifying people and exclusion rules in `people.json`. The final selections are saved to a json file.

## Why is this one so great??

It can be run locally (or wherever you want) and requires no registration or email addresses. The information is just for you and your people.

Theoretically you could game the seed to get your preferred person, but that would take all the fun out of it. Don't do that.

## 🎁 How it works

- On load the app reads `people.json`. Keep the files together!
- It attempts to generate a valid drawing by assigning each giver a receiver such that:
  - no one is assigned to themselves, and
  - assignments respect `exclusions` (if A excludes B, A will not be assigned B).
- If a valid assignment cannot be found after several attempts the app alerts you.

## 🎁 Seed configuration

The app supports a configurable random seed. Seeds are provided in these two ways:
1. Add a top-level `seed` field in `people.json` (number or string). The app will use this seed on load.
2. Do nothing and a seed will be provided for you.

### Why?

Computers can't actually do random numbers, so we have to get as close as we can to "random". The `seed` value controls the random generator used for shuffling and pair generation. Using the same `seed` (and the same `people.json`) will produce the same assignments and order every time you reload the page. You can use this to reproduce a draw or to share a fixed draw configuration with others.

### Notes about defaults and the UI:

If `people.json` does not include a `seed` field, the app uses the current year as a default seed on load (this gives a reproducible default run). 

## 🎁 Quick start (local)

1. Make sure `gift_draw.html` and `people.json` are in the same folder.
2. Open `gift_draw.html` in your browser (double-click or right-click → Open With → your browser). For full functionality (fetching `people.json`) you may need to serve the folder via a simple static server instead of opening the file URL in some browsers.

## 🎁 Serve locally with Python (recommended if the file fetch doesn't work):

```Toph
# from the repository folder
python3 -m http.server 8000
# then open http://localhost:8000/gift_draw.html
```

## 🎁 Usage

- Click "Next Turn" to step through givers. The app will enable "Spin" for the current giver.
- Click "Spin" to play the slot-style reveal for the assigned receiver.
- When everyone has finished, the app automatically downloads `selections.json` containing the final mapping of giver -> receiver.

## 🎁 Files
- `gift_draw.html` — The full client-side web app (HTML, CSS, JS). Open this in a browser to run the draw.
- `people.json` — Input data (list of people and an `exclusions` map). See the example in the file.
- `README.md` — This file.

## 🎁 Data format (`people.json`)

The file is a JSON object with two top-level keys:

- `people`: an array of names (strings). These are the participants and must be unique.
- `exclusions`: an object where each key is a giver name and its value is an array of names that the giver must not be assigned to. If a giver has no exclusions, they can be omitted or mapped to an empty array.

### people.json example

```json
{
	"seed": 2025,
	"people": ["Aang", "Katara", "Sokka", "Zuko", "Iroh", "Toph"],
	"exclusions": {
		"Aang": ["Katara", "Sokka"],
		"Katara": ["Aang", "Iroh"],
		"Sokka": ["Katara"],
		"Zuko": ["Toph"],
		"Iroh": ["Aang"],
		"Toph": ["Zuko"]
	}
}
```

## 🎁 Notes and troubleshooting

- If the app reports it cannot find a valid draw, try loosening exclusions or adding more participants. The algorithm tries random permutations up to a fixed number of attempts.
- When opening `gift_draw.html` via the `file://` protocol some browsers block the script from fetching `people.json` — run a local server instead (see Quick start).
- The app seeds its random generator (there's a seed in the file) so results can be reproducible across runs; edit `gift_draw.html` to change or remove the seed for more randomness.

## 🎁 License

This project is licensed under the MIT License — see the `LICENSE` file for details.

## 🎁 Contact

Open an issue if you find a bug.

