# Interests Network Graph

Hosted version: https://interests-network-graph.shashwatv.com/

Project by Shashwat Verma.

License: MIT (see LICENSE).

## Tech stack

![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwind-css&logoColor=white)
![vis-network](https://img.shields.io/badge/vis--network-2563EB?style=for-the-badge&logo=javascript&logoColor=white)
![jsPDF](https://img.shields.io/badge/jsPDF-F59E0B?style=for-the-badge&logo=adobeacrobatreader&logoColor=white)
![JSON Data](https://img.shields.io/badge/Meta_Data_JSON-0EA5E9?style=for-the-badge&logo=json&logoColor=white)

Interests Network Graph was inspired by a talk on user embeddings. This experimental project helps you visualize your own exported ads/interests data from Instagram or Facebook as a network graph.

## What this project does

- Loads structured interests data and renders an interactive network graph.
- Groups interests into clusters.
- Highlights hub nodes and cross-cluster relationships.
- Lets you export the graph as PNG or PDF.
- Lets you paste JSON or upload a JSON file in the app.

## How to get your Meta data export (Instagram/Facebook)

Use Meta Accounts Center to request your information download.

1. Go to Accounts Center in Instagram or Facebook.
2. Open Your information and permissions.
3. Choose Download your information.
4. Select your account (Instagram or Facebook).
5. Choose specific types of information and include ads-related data.
6. Choose JSON format when possible (important).
7. Download and extract the archive when Meta notifies you on the mail.

Important data area to find:

- ads_information

Instagram commonly contains files under:

- ads_information/instagram_ads_and_businesses/
- Example path:
	ads_information/instagram_ads_and_businesses/advertisers_using_your_activity_or_information.json

Facebook commonly contains:
- ads_information/advertisers_using_your_activity_or_information

## Convert export file to graph JSON (Gemini)

Use Gemini to transform your raw export file into the schema this project expects.

Recommended model: Gemini Pro 3.1

Important:

- Upload your exported file directly to Gemini (do not copy/paste huge raw content).
- Save Gemini's response as a `.json` or just copy the JSON part and paste it in the app.

### Prompt to get the structured data. Use this prompt to get a clean JSON output for the graph:

```markdown
You are a data transformation assistant. Parse the uploaded file and convert it into a clean JSON for an interactive network graph.

This particular data is of an anon user from instagram/facebook, in list/JSON format. The data defines the engagement with the mentioned accounts/people/organizations/companies and institutions. Some of the datapoints/names are not be legible or have semantic meaning associated, so omit them in the result output. 

Using the datapoints we want to make correlations/connections for vizualization. The below is the output format (should stricly follow the standard JSON formating along with key-value pairs, do not furnish any data information yourself. adhere to the rules)

create relationships and maximum 10 clusters based on the datapoints (keep optimium hubs, maximum hubs per cluster 4-6) omit the duplicates and remove the datapoints which are not legible or ambiguous but try to keep the maximum nodes, this JSON will be used for creating graph-node relationships. The cluster name should be based on the unique user datapoints and may vary based off that. Output result must only be exact key-value pairs. Each cluster must have a color associated (unique) to it. Each cluster must have the max amount of nodes possible as per the legible data points. First give the list of datapoints (comma separated) then the JSON output.

output format (strict format in JSON):-
{
  "clusters": {
    "1": { "name": "Advertising & Marketing", "color": "#ef4444" },
    "2": { "name": "Hospitality & Hotels", "color": "#f97316" },
    "3": { "name": "AI & Technology", "color": "#22c55e" },
    "4": { "name": "Fashion, Retail & D2C", "color": "#ec4899" },
    "5": { "name": "Education & EdTech", "color": "#8b5cf6" },
    "6": { "name": "Airlines & Travel", "color": "#06b6d4" },
    "7": { "name": "Finance & FinTech", "color": "#eab308" },
    "8": { "name": "Media & Entertainment", "color": "#3b82f6" },
    "9": { "name": "Wellness & Lifestyle", "color": "#14b8a6" },
    "10": { "name": "Enterprise & Conglomerates", "color": "#6366f1" }
  },
  "crossClusterConnections": [
    { "cluster1": 6, "cluster2": 2 },
    { "cluster1": 1, "cluster2": 8 },
    { "cluster1": 3, "cluster2": 10 },
    { "cluster1": 4, "cluster2": 1 },
    { "cluster1": 7, "cluster2": 10 },
    { "cluster1": 9, "cluster2": 4 },
    { "cluster1": 5, "cluster2": 3 },
    { "cluster1": 8, "cluster2": 10 }

  ],
  "interests": [
    { "label": "Dentsu Singapore", "group": 1, "isHub": true },
    { "label": "Hopscotch Europe", "group": 1 },
    { "label": "Dentsu UK", "group": 1, "isHub": true },
    { "label": "Social Beat", "group": 1 },
    { "label": "Omnicom Media", "group": 1, "isHub": true },
    { "label": "M&C Saatchi", "group": 1 },
    { "label": "Ogilvy", "group": 1, "isHub": true },
    { "label": "Publicis Groupe", "group": 1, "isHub": true },
    { "label": "GroupM", "group": 1, "isHub": true },
    { "label": "Starcom", "group": 1 },
    { "label": "Performics", "group": 1 },
    { "label": "Zenith", "group": 1 },
.......

.......

    { "label": "McKinsey & Company", "group": 10, "isHub": true },
    { "label": "LinkedIn", "group": 10, "isHub": true },
    { "label": "Deloitte", "group": 10, "isHub": true },
    { "label": "BCG", "group": 10 },
    { "label": "EY", "group": 10 },
    { "label": "Reliance Industries", "group": 10, "isHub": true },
    { "label": "Nestlé", "group": 10 },
    { "label": "Tesla", "group": 10, "isHub": true },
    { "label": "SpaceX", "group": 10 },
    { "label": "Rivian", "group": 10 }
  ]
}

Return ONLY valid JSON in this exact schema:
{
	"clusters": {
		"1": { "name": "Cluster Name", "color": "#hex" },
		"2": { "name": "Cluster Name", "color": "#hex" }
	},
	"crossClusterConnections": [
		{ "cluster1": 1, "cluster2": 2 }
	],
	"interests": [
		{ "label": "Interest or advertiser", "group": 1, "isHub": true },
		{ "label": "Another interest", "group": 2 }
	]
}
Raw data/list/HTML/JSON attached/below.

Rules:
- Create 5-10 meaningful clusters.
- Keep cluster ids numeric strings starting from "1".
- Every interest must include `label` and `group`.
- `group` must map to an existing cluster id.
- Mark only key representative nodes as `isHub: true`.
- Add `crossClusterConnections` only when relationship is meaningful.
- Use distinct, readable hex colors for clusters.
- Output strict JSON only. No markdown, no explanation.
```

## Use the app

1. Open https://interests-network-graph.shashwatv.com/
2. Click Load JSON.
3. Upload your converted `.json` file (recommended) or paste JSON.
4. Click Validate JSON.
5. Click Render Graph.
6. Explore clusters, hubs, and connections.
7. Export PNG/PDF if needed.

## Local development

This is a static project. No server-side dependencies required. Built with:
- **Tailwind CSS** - UI styling and utility classes
- **vis JS** - Interactive network graph rendering and physics layout
- **jsPDF** - PDF export functionality

## Notes
- This project is experimental.
- No data is sent to the server, also keep your personal export files private.
- Further expanding on the concept of user embeddings, a discussion on some open protocol or standard for user data portability and use in third-party applications would be interesting. This project is a small step in that direction, but there is much more to explore and build in this space.
- This project is not affiliated with Meta or any other company. It is a personal project for data visualization and exploration.