<div class="nested0" aria-labelledby="ariaid-title1" topicindex="1" topicid="lam1507582387267" id="lam1507582387267"><h1 class="title topictitle1" id="ariaid-title1">NamedEntityFinder (ML Engine)</h1><div class="body conbody">
<p class="p">The NamedEntityFinder function evaluates the input, identifies tokens based on the specified model, and outputs the tokens with detailed information. The function does not identify sentences; it simply tokenizes. Token identification is not case-sensitive.</p>
<p class="p">NamedEntityFinder uses files that are preinstalled on <span><b>ML Engine</b></span>. For details, see <a href="tzu1557778477026.md">Preinstalled Files That Functions Use</a>.</p></div><div class="topic reference nested1" aria-labelledby="ariaid-title2" topicindex="2" topicid="fsr1507582452484" xml:lang="en-us" lang="en-us" id="fsr1507582452484">
<h2 class="title topictitle2" id="ariaid-title2">NamedEntityFinder Syntax</h2><div class="body refbody"><div class="section" id="fsr1507582452484__section_N1000E_N1000C_N10001">
<h3 class="title sectiontitle">Version <span>1.6</span></h3><pre class="pre codeblock" xml:space="preserve"><code>SELECT * FROM NamedEntityFinder (
  <span>ON { <var class="keyword varname">table</var> | <var class="keyword varname">view</var> | (<var class="keyword varname">query</var>) }</span> PARTITION BY ANY
  [ ON (<var class="keyword varname">configure_table</var>) AS ConfigurationTable DIMENSION ]
  USING
  TextColumn ('<var class="keyword varname">text_column</var>')
  [ Models ('<var class="keyword varname">entity_type</var>[:<var class="keyword varname">model_type</var>:{<var class="keyword varname">model_file</var>|<var class="keyword varname">regular_expression</var>}'][,...] | 'all' }) ]
  [ ShowContext ('<var class="keyword varname">context_words</var>') ]
  [ EntityColName ('<var class="keyword varname">entity_column</var>') ]
  <code class="ph codeph">[ Accumulate ({ '<var class="keyword varname">accumulate_column</var>' | <var class="keyword varname">accumulate_column_range</var> }[,...]) ]</code>
) AS <var class="keyword varname">alias</var>;</code></pre></div></div><div class="related-links"><div class="linklistheader"><p></p><b>Related Information</b></div>
<ul class="linklist linklist"><div class="linklistmember"><a href="ndv1557782188375.md">Column Specification Syntax Elements</a></div><div class="linklistmember"><a href="dsd1557781660424.md">Regular Expressions in Syntax Elements</a></div></ul></div></div><div class="topic reference nested1" aria-labelledby="ariaid-title3" topicindex="3" topicid="rdh1507582703200" xml:lang="en-us" lang="en-us" id="rdh1507582703200">
<h2 class="title topictitle2" id="ariaid-title3">NamedEntityFinder Syntax Elements</h2><div class="body refbody"><div class="section" id="rdh1507582703200__section_N10011_N1000E_N10001"><dl class="dl parml"><dt class="dt pt dlterm">TextColumn</dt><dd class="dd pd">Specify the name of the input table column that contains the text to analyze.</dd><dt class="dt pt dlterm">Models</dt><dd class="dd pd">[Optional] Required if you do not specify ConfigurationTable, in which case you cannot specify 'all'. Specify the model items to load.</dd><dd class="dd pd ddexpand">If you specify both ConfigurationTable and this syntax element, the function loads the specified model items from ConfigurationTable.</dd><dd class="dd pd ddexpand">The <var class="keyword varname">entity_type</var> is the name of an entity type (for example, PERSON, LOCATION, or EMAIL), which appears in the output table.</dd><dd class="dd pd ddexpand"><div class="tablenoborder"><table cellpadding="4" cellspacing="0" summary="" id="rdh1507582703200__table_qn5_h1z_fdb" class="table" frame="border" border="1" rules="all"><div class="caption"></div><colgroup span="1"><col style="width:50%" span="1"></col><col style="width:50%" span="1"></col></colgroup><thead class="thead" style="text-align:left;"><tr class="row"><th class="entry cellrowborder" style="vertical-align:top;" id="d35140e193" rowspan="1" colspan="1"><var class="keyword varname">model_type</var></th><th class="entry cellrowborder" style="vertical-align:top;" id="d35140e196" rowspan="1" colspan="1">Description</th></tr></thead><tbody class="tbody"><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e193" rowspan="1" colspan="1"><code class="ph codeph">'max entropy'</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e196" rowspan="1" colspan="1">Maximum entropy language model output by training.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e193" rowspan="1" colspan="1"><code class="ph codeph">'rule'</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e196" rowspan="1" colspan="1">Rule-based model, a plain text file with one regular expression on each line.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e193" rowspan="1" colspan="1"><code class="ph codeph">'dictionary'</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e196" rowspan="1" colspan="1">Dictionary-based model, a plain text file with one word on each line.</td></tr><tr class="row"><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e193" rowspan="1" colspan="1"><code class="ph codeph">'reg exp'</code></td><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e196" rowspan="1" colspan="1">Regular expression that describes <var class="keyword varname">entity_type</var>.</td></tr></tbody></table></div></dd><dd class="dd pd ddexpand">If <var class="keyword varname">model_type</var> is 'reg exp', specify <var class="keyword varname">regular_expression</var> (a regular expression that describes <var class="keyword varname">entity_type</var>); otherwise, specify <var class="keyword varname">model_file</var> (the name of the model file).</dd><dd class="dd pd ddexpand">
<p class="p">If you specify ConfigurationTable, you can use <var class="keyword varname">entity_type</var> as a shortcut. For example, if the ConfigurationTable has the row 'organization, max entropy, en-ner-organization.bin', you can specify Models ('organization') as a shortcut for Models ('organization:max entropy:en-ner-organization.bin').</p><div class="note note" id="rdh1507582703200__note_N100AC_N1004E_N10042_N10030_N10011_N1000E_N1000C_N10001"><span><b>Note</b></span><div class="notebody">For <var class="keyword varname">model_type</var> 'max entropy', if you specify ConfigurationTable and omit this syntax element, then the JVM of the worker node needs more than 2GB of memory.</div></div></dd><dd class="dd pd ddexpand">Default: 'all' (If you specify ConfigurationTable but omit this syntax element.)</dd><dt class="dt pt dlterm">ShowContext</dt><dd class="dd pd">[Optional] Specify the number of context words to output. If <var class="keyword varname">context_words</var> is n (which must be a positive integer), the function outputs the n words that precede the entity, the entity, and the <var class="keyword varname">n</var> words that follow the entity.</dd><dd class="dd pd ddexpand">Default: 0</dd><dt class="dt pt dlterm">EntityColName</dt><dd class="dd pd">[Optional] Specify the name of the output table column that contains the entity names.</dd><dd class="dd pd ddexpand">Default: 'entity'</dd><dt class="dt pt dlterm">Accumulate</dt><dd class="dd pd">[Optional] Specify the names of input columns to copy to the output table. No <var class="keyword varname">accumulate_column</var> can be an <var class="keyword varname">entity_column</var>.</dd><dd class="dd pd ddexpand">Default: All input columns</dd></dl></div></div></div><div class="topic reference nested1" aria-labelledby="ariaid-title4" topicindex="4" topicid="bvn1507584767132" xml:lang="en-us" lang="en-us" id="bvn1507584767132">
<h2 class="title topictitle2" id="ariaid-title4">Creating the Table of Default Models</h2><div class="body refbody"><div class="section" id="bvn1507584767132__section_N1000E_N1000C_N10001">
<p class="p">Before calling the NamedEntityFinder function, you must create the table of default models. To create the table, use this command:</p><pre class="pre codeblock" xml:space="preserve"><code>DROP TABLE nameFind_configure;

CREATE MULTISET TABLE nameFind_configure (
  model_name VARCHAR(50),
  model_type VARCHAR(50),
  model_file VARCHAR(50)
);</code></pre>
<p class="p">Default English-language models are provided with the <span><b>SQL</b></span> functions. Before using these models, you must create a default configure_table, as follows:</p><pre class="pre codeblock" xml:space="preserve"><code>INSERT INTO nameFind_configure VALUES ('person','max entropy','en-ner-person.bin');
INSERT INTO nameFind_configure VALUES ('location','max entropy','en-ner-location.bin');
INSERT INTO nameFind_configure VALUES ('organization','max entropy','en-ner-organization.bin');
INSERT INTO nameFind_configure VALUES ('date','rules','date.rules');
INSERT INTO nameFind_configure VALUES ('time','rules','time.rules');
INSERT INTO nameFind_configure VALUES ('phone','rules','phone.rules');
INSERT INTO nameFind_configure VALUES ('money','rules','money.rules');
INSERT INTO nameFind_configure VALUES ('email','rules','email.rules');
INSERT INTO nameFind_configure VALUES ('percentage','rules','percentage.rules');
</code></pre><div class="tablenoborder"><table cellpadding="4" cellspacing="0" summary="" id="bvn1507584767132__table_N1001F_N1000E_N1000C_N10001" class="table" frame="border" border="1" rules="all"><div class="caption"><span>Default English-Language Models in Table nameFind_configure</span></div><colgroup span="1"><col style="width:28.57142857142857%" span="1"></col><col style="width:28.57142857142857%" span="1"></col><col style="width:42.857142857142854%" span="1"></col></colgroup><thead class="thead" style="text-align:left;"><tr class="row"><th class="entry nocellnorowborder" style="vertical-align:top;" id="d35140e319" rowspan="1" colspan="1">model_name</th><th class="entry nocellnorowborder" style="vertical-align:top;" id="d35140e321" rowspan="1" colspan="1">model_type</th><th class="entry cell-norowborder" style="vertical-align:top;" id="d35140e323" rowspan="1" colspan="1">model_file</th></tr></thead><tbody class="tbody"><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">person</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">max entropy</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">en-ner-person.bin</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">location</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">max entropy</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">en-ner-location.bin</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">organization</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">max entropy</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">en-ner-organization.bin</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">date</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">date.rules</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">time</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">time.rules</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">phone</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">phone.rules</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">money</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">money.rules</td></tr><tr class="row"><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">email</td><td class="entry nocellnorowborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cell-norowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">email.rules</td></tr><tr class="row"><td class="entry row-nocellborder" style="vertical-align:top;" headers="d35140e319" rowspan="1" colspan="1">percentage</td><td class="entry row-nocellborder" style="vertical-align:top;" headers="d35140e321" rowspan="1" colspan="1">rules</td><td class="entry cellrowborder" style="vertical-align:top;" headers="d35140e323" rowspan="1" colspan="1">percentage.rules</td></tr></tbody></table></div></div></div></div></div>
