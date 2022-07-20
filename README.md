# Git Graph js example
A JavaScript [library](https://www.nicoespeon.com/gitgraph.js/#0) to draw pretty git graphs.

![Git Graph js example](https://github.com/daljitdokal/git-graph-js-example/blob/main/example.PNG)

```html
<html>
  <head>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/gitgraph.js/1.11.4/gitgraph.css" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gitgraph.js/1.11.4/gitgraph.min.js"></script>
	<style>*{font-family:Arial;}</style>
  </head>
  <body><canvas id="graph"></canvas></body>
  
  <script>

	const main = {
	  prod:    {index: 2, name: "prod"},
	  stag:    {index: 3, name: "stag"},
	  test:    {index: 4, name: "test"},
	  dev:     { index: 5, name: "dev" },
	  feature: { index: 6, name: "feature"},
	  bugfix:  { index: 7, name: "bugfix" },
	  hotfix:  { index: 1, name: "hotfix" }
	};
	const myTemplateConfig = {
	  colors: [ "#f28e2c", "#e15759", "#59a14f", "#76b7b2", "#af7aa1", "#edc949", "#bab0ab", "#9c755f", "#bab0ab" ],
	  branch: { lineWidth: 8, spacingX: 50, showLabel: true, labelFont: "normal 14pt Courier New", labelRotation: 0},
	  commit: { spacingY: -40, dot: { size: 12 },message: { displayAuthor: true, displayBranch: true, displayHash: false, font: "normal 12pt Arial" }, shouldDisplayTooltipsInCompactMode: "true", tooltipHTMLFormatter: function (commit) { return "" + commit.sha1 + "" + ": " + commit.message; } } 
	};
	  
	var myTemplate = new GitGraph.Template(myTemplateConfig);

	const gitgraph = new GitGraph({ orientation: "vertical", author: "Daljit Singh<daljitdokal@yahoo.co.nz>", elementId: "graph", initCommitOffsetX: 0, initCommitOffsetY: 0, mode: "extended", template: myTemplate });

	const prod = gitgraph.branch({name: "production", column: main["prod"]["index"]}).commit({ message: "Initial Commit" });
	const stag= gitgraph.branch({parentBranch: prod, name: "staging", column: main["stag"]["index"]}).commit({ message: "Branch created" });
	const test = gitgraph.branch({parentBranch: stag, name: "test", column: main["test"]["index"]}).commit({ message: "Branch created" });
	const dev = gitgraph.branch({parentBranch: test, name: "development", column: main["dev"]["index"]}).commit({ message: "Branch created" });


	const devCommit = {messageDisplay: false,};
	dev.commit(devCommit);

	const hotFix = function (name, parent) {
	  return gitgraph.branch({parentBranch: parent, name: name, column: main["hotfix"]["index"]});
	};

	const addFeature = function (name) {
	  return gitgraph.branch({
		parentBranch: dev,
		name: `feature/${name}`,
		column: main["feature"]["index"]
	  });
	};

	const aFeature = addFeature("new-feature").commit("Starting work on feature for next release");
	aFeature.merge(dev);
	dev.merge(test);
	test.merge(stag);
	stag.merge(prod);


	stag.commit("Using as").tag("Tag-stg v2.0");
	test.commit("Using as").tag("Tag-test v2.0");
	dev.commit("Using as").tag("Tag-dev v2.0");
	prod.commit("Using as").tag("Tag-prd v1.0");


	const aHotFix = hotFix("hotfix/bug-fix", prod).commit("Hotfix for production bug");
	aHotFix.merge(prod);
	prod.merge(stag);
	stag.merge(test);
	test.merge(dev);


	dev.commit(devCommit);
	const bFeature = addFeature("another-feature").commit("code updated");
	bFeature.merge(dev);
	dev.merge(test);
	test.merge(stag);

	<!-- stag.merge(prod); -->

  </script>
</html>

```
