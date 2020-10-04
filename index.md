# Curse of Aros - All Ranks

<form onsubmit="search()">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" value=""/>
  <input type="submit" value="Find"/>
</form>
<table id="table">
  <tr>
    <th>Name</th>
    <th>Rank</th>
    <th>XP</th>
    <th>Page</th>
  </tr>
  <tr>
    <td class="left">XP</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Mining</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Smithing</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Woodcutting</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Crafting</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Fishing</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td class="left">Cooking</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

<p>Made by <a href="https://kaue.me">https://kaue.me</a></p>

<p>Data from <a href="https://curseofaros.com">https://curseofaros.com</a></p>

<style>

table {
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 2px;
}

td {
  text-align: right;
}

.left {
  text-align: left;
}

</style>

<script>

const MAX_PAGE = 1000;
const DEFAULT_NAME = "EXP Babyfaced";
const INDEX = {
  "rank": 1,
  "xp": 2,
  "page": 3,
};
const skills = ["", "mining", "smithing", "woodcutting", "crafting", "fishing", "cooking"];

function byId(id) {
  return document.getElementById(id);
}

function setCell(row, index, value) {
  row.cells[INDEX[index]].innerHTML = value;
}

let search = async () => {
  const name = byId("name").value;
  const table = byId("table");

  for (let i = 0; i < skills.length; i++) {
    const row = table.rows[i + 1];
    let suffix = skills[i];
    if (suffix != "") {
      suffix = "-" + suffix;
    }
    let found = false;
    let rank = 0;
    for (let page = 0; page < MAX_PAGE; page++) {
      setCell(row, "page", page + 1);
      const url = "https://www.curseofaros.com/highscores" + suffix + ".json?p=" + page;
      const response = await fetch(url);
      const json = await response.json();
      for (let i = 0; i < json.length; i++) {
        rank++;
        const item = json[i];
        if (item.name != name) {
          continue
        }
        setCell(row, "rank", rank);
        setCell(row, "xp", item.xp);
        found = true;
        break
      }
      if (found) {
        break;
      }
    }
    if (!found) {
      console.log("name", name, "not found");
    }
  }
};

function main() {
  let name = DEFAULT_NAME;
  const queryString = window.location.search;
  const urlParams = new URLSearchParams(queryString);
  if (urlParams.has("name")) {
    got = urlParams.get("name");
    if (got != "") {
      name = got;
    }
  }
  byId("name").value = name;
  search();
}

main();

</script>
