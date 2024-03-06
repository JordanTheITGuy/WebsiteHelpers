<%*
const dv = app.plugins.plugins["dataview"].api;

// Add as many filenames and queries as you'd like!
const fileAndQuery = new Map([
  [
    "Home Output",
'TABLE WITHOUT ID ("![[]](" + cover + ")") as Cover,file.link AS Post,(dateformat(date, "yyyy-MM-dd")) AS Modified, description as "Description" WHERE publish = "true" and exclude != "true" SORT date desc',
  ]
]);

await fileAndQuery.forEach(async (query, filename) => {
  if (!tp.file.find_tfile(filename)) {
    await tp.file.create_new("", filename);
    new Notice(`Created ${filename}.`);
  }
  const tFile = tp.file.find_tfile(filename);
  const queryOutput = await dv.queryMarkdown(query);
  const fileContent = `\n\n${queryOutput.value}`;
  try {
    await app.vault.modify(tFile, fileContent);
    new Notice(`Updated ${tFile.basename}.`);
  } catch (error) {
    new Notice("⚠️ ERROR updating! Check console. Skipped file: " + filename , 0);
  }
});
%>
