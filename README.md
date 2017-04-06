# api documentation for  gramophone (v0.0.3)  [![npm package](https://img.shields.io/npm/v/npmdoc-gramophone.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gramophone) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gramophone.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gramophone)
#### extracts most frequently used keywords and phrases from text

[![NPM](https://nodei.co/npm/gramophone.png?downloads=true)](https://www.npmjs.com/package/gramophone)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gramophone/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gramophone_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gramophone/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gramophone/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gramophone/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "dependencies": {
        "event-stream": "~3.0.8",
        "lodash": "~0.8.2",
        "natural": "~0.1.18",
        "underscore.string": "~2.3.1"
    },
    "description": "extracts most frequently used keywords and phrases from text",
    "devDependencies": {
        "tap": "~0.3.3"
    },
    "directories": {},
    "dist": {
        "shasum": "1003e27f8eacd4dbc61155555fda3e16d52c1e64",
        "tarball": "https://registry.npmjs.org/gramophone/-/gramophone-0.0.3.tgz"
    },
    "keywords": [
        "keyword",
        "keywords",
        "natural language",
        "npl"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "bxjx",
            "email": "b.j.rossiter@gmail.com"
        }
    ],
    "name": "gramophone",
    "optionalDependencies": {},
    "readme": "Gramophone\n==========\n\n[![Build Status](https://secure.travis-ci.org/bxjx/gramophone.png?branch=master)](https://travis-ci.org/bxjx/gramophone)\n\nExtracts most frequently used keywords and phrases from text. It excludes\ncommon stop words. It can be configured to extract arbitary length phrases\n(ngrams) rather than just keywords.\n\n'''js\nrequest('https://github.com/substack/stream-handbook/blob/master/readme.markdown')\n  .pipe(gramophone.stream({ngrams: 2, html: true, limit: 2}))\n  .on('data', console.error.bind(console));\n'''\n\nWould write out:\n'''\nreadable stream\nwritable stream\n'''\n\nAPI\n---\n\n  * <a href=\"#extract\"><code>gramophone.<b>extract()</b></code></a>\n  * <a href=\"#stream\"><code>gramophone.<b>stream()</b></code></a>\n  * <a href=\"#transformStream\"><code>gramophone.<b>transformStream()</b></code></a>\n\n--------------------------------------------------------\n<a name=\"extract\"></a>\n### gramophone.extract(text[, options])\n\nSynchronously extracts keywords from the text. By\ndefault it returns any keyword phrases that occur more than once. It also\nremoves any common English words. It returns the results reverse ordered by\nfrequency i.e. the first result is the most common phrase.\n\n'''js\nkeyword.extract('beep beep and foo bar and beep beep and beep beep and foo bar')\n'''\n\nReturns '['beep beep', 'foo bar']'.\n\n#### Option: score\n\nReturns each keyword as an object where 'term' is the keyword and 'tf' is the\nnumber of times the phrase was used i.e. the term frequency. Off by default.\n\n'''js\nkeyword.extract('beep beep and foo bar and beep beep and beep beep and foo bar', {score: true})\n'''\n\nReturns '[{term: 'beep beep', tf: 3}, {term: 'foo bar', tf: 2}]'.\n\n#### Option: limit\n\nReturns the top N results. The default is to not limit the results.\n\n'''js\nkeyword.extract('beep beep and foo bar and beep beep and beep beep and foo bar', {limit: 1})\n'''\n\nReturns '['beep beep']'.\n\n#### Option: flatten\n\nReturns all occurrences of the ngram. Useful for passing data to Natural's\nTF-IDF function. Note: the original order is not maintained. Off by default.\n\n'''js\nkeyword.extract('beep beep and foo bar and beep beep and beep beep and foo bar', {flaten: true})\n'''\n\nReturns '['beep beep', 'beep beep', 'beep beep', 'foo bar', 'foo bar']'.\n\n#### Option: html\n\nExtracts the keywords from html text elements. The default is false.\n\n'''js\nkeyword.extract('<strong>beep</strong>, <strong>beep</strong> and <strong>foo</strong>', {html: true})\n'''\n\nReturns '['beep', 'foo']'.\n\n#### Option: min\n\nOnly returns results with greater than or equal to N occurences. The default value is 2.\n\n'''js\nkeyword.extract('beep and beep and beep and foo and foo', {min: 3})\n'''\n\nReturns '['beep']'.\n\n#### Option: ngrams\n\nIf ngrams is a number (N), only look for phrases with N words. If ngrams is\na list ([N1, N2]), only look for the phrases with N1 or N2 words etc.. The\ndefualt is too look for [1, 2, 3] word ngrams.\n\n'''js\nkeyword.extract('beep and beep and beep bop boop and foo and foo bar', {ngrams: [2, 3]})\n'''\n\nReturns '['beep bop boop', 'foo bar']'.\n\n#### Option: stopWords\n\nAdd extra stopWords to be used in addition to the English set.\n\n'''js\nkeyword.extract('foo et bar et foo et bar et foo', {stopWords: ['et']})\n'''\n\nReturns '['foo', 'bar']'.\n\n#### Option: startWords\n\nAny words in this list are whitelisted even if they are a stop word.\n\n'''js\nkeyword.extract('foo and bar with foo and bar', {startWords: ['and']})\n'''\n\nReturns '['foo and bar']'\n\n#### Option: stem\n\nApply stemming before extracting keywords. The returned keyword will be the\nmost frequently used word.\n\n'''js\nkeyword.extract('fooing and foo and fooing', {stem: true})\n'''\n\nReturns '['fooing']'\n\n#### Option: cutoff\n\nAllows you to specify the cutoff for determining whether to include a phrase\nthat is a component of another phrase. E.g. should \"node\" and \"runs\" be\nextracted as keywords as well as \"node runs\".\n\nA component phrase is filtered based on the following formula:\n\n' phrase freq. / component phrase freq. >= 1 - cutoff'\n\nE.g., let's say you have some text that includes the phrase \"node runs\" 20 times,\n\"node\" 40 times and \"runs\" 22 times. If the cutoff was 0.5 (the default),\n\"node\" would be included as '20 / 40 >= 1 - 0.5'. However, \"runs\" would not\nbe returned as a keyword as '20 / 22 < 1 - 0.5'.\n\nWow. I could probably make this more intuitive. Open to suggestions.\n\n--------------------------------------------------------\n<a name=\"stream\"></a>\n### gramophone.stream([options])\n\nReturns a through stream that reads in the text stream and emits keywords\nbased on the options passed. It uses the same options as 'extract'. Note: this\nstream behaves like a sink and will buffer the stream completely before emitting\nkeywords.\n\nSee first example.\n\n--------------------------------------------------------\n<a name=\"transformStream\"></a>\n### gramophone.transformStream([options])\n\nReturns a through stream that reads in the stream and emits keywords for each\ndata read. By default, it assumes that each data read in a string. Alternatively\nthe stream can read and write to objects. To read\nthe text from an object property, specify the 'from' option. If you want to\nwrite the keywords back to the object, also specify the 'to' option.\n\n'''js\nvar stream = gramophone.transformStream({from: 'text', to: 'keywords'});\nstream.write({ text: 'foo and bar and foo'});\nstream.end();\n'''\n\nEmits the data: '{ text: 'foo and bar and foo', keywords: [foo] }'.\n\nRelated projects\n----------------\n\n  * [node-alchemy](https://github.com/framingeinstein/node-alchemy): \n    a cloud based keyword extraction service.\n  * [natural](https://github.com/NaturalNode/natural): a fantastic natural\n    language processing library for node.js. Checkout\n    [Tf-Idf](https://github.com/NaturalNode/natural#tf-idf) if you're looking\n    to extract keywords based on their relative frequency to other documents.\n    If people are interested, I might add Tf-Idf support to gramophone.\n\nLicence & copyright\n-------------------\n\ngramophone is Copyright (c) 2012 B.J. Rossiter.\n\ngramophone is licensed under the MIT licence. All rights not explicitly granted in the MIT license are reserved. See the included LICENSE file for more details.\n",
    "readmeFilename": "README.md",
    "scripts": {
        "test": "./node_modules/.bin/tap test/*"
    },
    "version": "0.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gramophone](#apidoc.module.gramophone)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>TextStream (options)](#apidoc.element.gramophone.TextStream)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>combine (phrases, cutoff)](#apidoc.element.gramophone.combine)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>extract (text, options)](#apidoc.element.gramophone.extract)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>stream (options)](#apidoc.element.gramophone.stream)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>transformStream (options)](#apidoc.element.gramophone.transformStream)
1.  object <span class="apidocSignatureSpan">gramophone.</span>TextStream.prototype

#### [module gramophone.TextStream](#apidoc.module.gramophone.TextStream)
1.  [function <span class="apidocSignatureSpan">gramophone.</span>TextStream (options)](#apidoc.element.gramophone.TextStream.TextStream)
1.  [function <span class="apidocSignatureSpan">gramophone.TextStream.</span>super_ ()](#apidoc.element.gramophone.TextStream.super_)

#### [module gramophone.TextStream.prototype](#apidoc.module.gramophone.TextStream.prototype)
1.  [function <span class="apidocSignatureSpan">gramophone.TextStream.prototype.</span>end (data)](#apidoc.element.gramophone.TextStream.prototype.end)
1.  [function <span class="apidocSignatureSpan">gramophone.TextStream.prototype.</span>write (data)](#apidoc.element.gramophone.TextStream.prototype.write)



# <a name="apidoc.module.gramophone"></a>[module gramophone](#apidoc.module.gramophone)

#### <a name="apidoc.element.gramophone.TextStream"></a>[function <span class="apidocSignatureSpan">gramophone.</span>TextStream (options)](#apidoc.element.gramophone.TextStream)
- description and source-code
```javascript
TextStream = function (options){
  this.options = options;
  this.readable = true;
  this.writable = true;
  this.buffer = [];
}
```
- example usage
```shell
...

return combined;
};

// Text stream. Reads a text stream and emits keywords. Warning: this stream
// behaves like a sink and will buffer all data until the source emits end.
exports.stream = function(options){
return new exports.TextStream(options);
};

exports.TextStream = function(options){
this.options = options;
this.readable = true;
this.writable = true;
this.buffer = [];
...
```

#### <a name="apidoc.element.gramophone.combine"></a>[function <span class="apidocSignatureSpan">gramophone.</span>combine (phrases, cutoff)](#apidoc.element.gramophone.combine)
- description and source-code
```javascript
combine = function (phrases, cutoff){
  var combined = _.clone(phrases);

  _.each(_.keys(phrases), function(phrase){
    var ngramToTry, subPhrases;
    ngramToTry = phrase.split(' ').length - 1;

    if (ngramToTry < 1) return;

    _.each(natural.NGrams.ngrams(phrase, ngramToTry), function(ngram){
      var subPhrase = ngram.join(' ');
      if (phrases[subPhrase]){
        if (!cutoff || (phrases[phrase] / phrases[subPhrase]) >= (1 - cutoff)){
          delete combined[subPhrase];
        }
      }
    });
  });

  return combined;
}
```
- example usage
```shell
...

// Convert results to a hash
_.each(results, function(result){
  combinedResults[result.term] = result.tf;
});

// Combine results from each ngram to remove redundancy phrases
combined = exports.combine(combinedResults, options.cutoff);

// Convert to a list of objects sorted by tf (term frequency)
combined = _.chain(combined)
  .pairs()
  .sortBy(_.last)
  .reverse()
  .map(function(combination){ return {term: combination[0], tf: combination[1] }; })
...
```

#### <a name="apidoc.element.gramophone.extract"></a>[function <span class="apidocSignatureSpan">gramophone.</span>extract (text, options)](#apidoc.element.gramophone.extract)
- description and source-code
```javascript
extract = function (text, options){
  var results = [];
  var keywords = {};
  var combined, combinedResults = {};
  var unstemmed = {};

  var stem = function(word){
    // only bother stemming if the word will be used
    if (!usePhrase(word, options)) return word;
    var stem = natural.PorterStemmer.stem(word);
    // Store the shortest word that matches this stem for later destemming
    if (!unstemmed.hasOwnProperty(stem) || word.length < unstemmed[stem].length){
      unstemmed[stem] = word;
    }
    return stem;
  };

  var destem = function(stem){
    return unstemmed[stem];
  };

  if (!text) return [];
  if (typeof text !== 'string') text = text.toString();

  if (!options) options = {};
  if (!options.ngrams){
    options.ngrams = [1, 2, 3];
  }else if (typeof options.ngrams === 'number'){
    options.ngrams = [options.ngrams];
  }
  if (!options.cutoff) options.cutoff = 0.5;
  if (!options.min) options.min = 2;
  if (!options.stopWords) options.stopWords = [];
  if (!options.startWords) options.startWords = [];
  if (options.html){
    text = stripTags(text);
  }

  // For each ngram, extract the most frequent phrases (taking into account
  // stop and start words lists)
  _.each(options.ngrams, function(ngram){
    var keywordsForNgram;
    var tf = new Tf();
    var tokenized = _.map(natural.NGrams.ngrams(text, ngram), function(ngram){
      if (options.stem){
        ngram = _.map(ngram, stem);
      }
      return ngram.join(' ').toLowerCase();
    });
    tf.addDocument(tokenized);
    keywordsForNgram = tf.listMostFrequestTerms(0);
    keywordsForNgram = _.select(keywordsForNgram, function(item){
      return usePhrase(item.term, options);
    });
    results = results.concat(keywordsForNgram);
  });

  // Convert results to a hash
  _.each(results, function(result){
    combinedResults[result.term] = result.tf;
  });

  // Combine results from each ngram to remove redundancy phrases
  combined = exports.combine(combinedResults, options.cutoff);

  // Convert to a list of objects sorted by tf (term frequency)
  combined = _.chain(combined)
    .pairs()
    .sortBy(_.last)
    .reverse()
    .map(function(combination){ return {term: combination[0], tf: combination[1] }; })
    .value();

  // Only return results over a given frequency (default is 2 or more)
  if (options.min){
    combined = _.select(combined, function(result){
      return result.tf >= options.min;
    });
  }

  // If stemming was used, remap words back
  if (options.stem){
    combined.forEach(function(result){
      result.term = _.map(result.term.split(' '), destem).join(' ');
    });
  }

  if (options.flatten){
    // Flatten the results so that there is a list item for every occurence of
    // the term
    combined = _.flatten(
      _.map(combined, function(result){
        var flattened = [];
        for (var i=0; i < result.tf; i++){
          flattened.push(result.term);
        }
        return flattened;
      })
    );
  }else{
    // Return results with scores or without depending on options
    combined =  options.score ? combined : _.pluck(combined, 'term');
  }


  // Limit the results
  if (options.limit){
    combined = combined.slice(0, options.limit);
  }

  return combined;
}
```
- example usage
```shell
...

  * <a href="#extract"><code>gramophone.<b>extract()</b></code></a>
  * <a href="#stream"><code>gramophone.<b>stream()</b></code></a>
  * <a href="#transformStream"><code>gramophone.<b>transformStream()</b></code></a>

--------------------------------------------------------
<a name="extract"></a>
### gramophone.extract(text[, options])

Synchronously extracts keywords from the text. By
default it returns any keyword phrases that occur more than once. It also
removes any common English words. It returns the results reverse ordered by
frequency i.e. the first result is the most common phrase.

'''js
...
```

#### <a name="apidoc.element.gramophone.stream"></a>[function <span class="apidocSignatureSpan">gramophone.</span>stream (options)](#apidoc.element.gramophone.stream)
- description and source-code
```javascript
stream = function (options){
  return new exports.TextStream(options);
}
```
- example usage
```shell
...

Extracts most frequently used keywords and phrases from text. It excludes
common stop words. It can be configured to extract arbitary length phrases
(ngrams) rather than just keywords.

'''js
request('https://github.com/substack/stream-handbook/blob/master/readme.markdown')
  .pipe(gramophone.stream({ngrams: 2, html: true, limit: 2}))
  .on('data', console.error.bind(console));
'''

Would write out:
'''
readable stream
writable stream
...
```

#### <a name="apidoc.element.gramophone.transformStream"></a>[function <span class="apidocSignatureSpan">gramophone.</span>transformStream (options)](#apidoc.element.gramophone.transformStream)
- description and source-code
```javascript
transformStream = function (options){
  if (!options) options = {};

  return es.through(function write(data){
    var from = options.from;
    var text = from && data.hasOwnProperty(from) ? data[from] : data;
    var keywords = exports.extract(text, options);
    if (options.to){
      data[options.to] = keywords;
      this.emit('data', data);
    }else{
      this.emit('data', keywords);
    }
  });

}
```
- example usage
```shell
...
stream behaves like a sink and will buffer the stream completely before emitting
keywords.

See first example.

--------------------------------------------------------
<a name="transformStream"></a>
### gramophone.transformStream([options])

Returns a through stream that reads in the stream and emits keywords for each
data read. By default, it assumes that each data read in a string. Alternatively
the stream can read and write to objects. To read
the text from an object property, specify the 'from' option. If you want to
write the keywords back to the object, also specify the 'to' option.
...
```



# <a name="apidoc.module.gramophone.TextStream"></a>[module gramophone.TextStream](#apidoc.module.gramophone.TextStream)

#### <a name="apidoc.element.gramophone.TextStream.TextStream"></a>[function <span class="apidocSignatureSpan">gramophone.</span>TextStream (options)](#apidoc.element.gramophone.TextStream.TextStream)
- description and source-code
```javascript
TextStream = function (options){
  this.options = options;
  this.readable = true;
  this.writable = true;
  this.buffer = [];
}
```
- example usage
```shell
...

return combined;
};

// Text stream. Reads a text stream and emits keywords. Warning: this stream
// behaves like a sink and will buffer all data until the source emits end.
exports.stream = function(options){
return new exports.TextStream(options);
};

exports.TextStream = function(options){
this.options = options;
this.readable = true;
this.writable = true;
this.buffer = [];
...
```

#### <a name="apidoc.element.gramophone.TextStream.super_"></a>[function <span class="apidocSignatureSpan">gramophone.TextStream.</span>super_ ()](#apidoc.element.gramophone.TextStream.super_)
- description and source-code
```javascript
function Stream() {
  EE.call(this);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.gramophone.TextStream.prototype"></a>[module gramophone.TextStream.prototype](#apidoc.module.gramophone.TextStream.prototype)

#### <a name="apidoc.element.gramophone.TextStream.prototype.end"></a>[function <span class="apidocSignatureSpan">gramophone.TextStream.prototype.</span>end (data)](#apidoc.element.gramophone.TextStream.prototype.end)
- description and source-code
```javascript
end = function (data){
  var stream = this;
  if (data) this.write(data);
  exports.extract(this.buffer.join(''), this.options).forEach(function(phrase){
    stream.emit('data', phrase);
  });
  stream.emit('end');
}
```
- example usage
```shell
...
the stream can read and write to objects. To read
the text from an object property, specify the 'from' option. If you want to
write the keywords back to the object, also specify the 'to' option.

'''js
var stream = gramophone.transformStream({from: 'text', to: 'keywords'});
stream.write({ text: 'foo and bar and foo'});
stream.end();
'''

Emits the data: '{ text: 'foo and bar and foo', keywords: [foo] }'.

Related projects
----------------
...
```

#### <a name="apidoc.element.gramophone.TextStream.prototype.write"></a>[function <span class="apidocSignatureSpan">gramophone.TextStream.prototype.</span>write (data)](#apidoc.element.gramophone.TextStream.prototype.write)
- description and source-code
```javascript
write = function (data){
  this.buffer.push(data);
}
```
- example usage
```shell
...
data read. By default, it assumes that each data read in a string. Alternatively
the stream can read and write to objects. To read
the text from an object property, specify the 'from' option. If you want to
write the keywords back to the object, also specify the 'to' option.

'''js
var stream = gramophone.transformStream({from: 'text', to: 'keywords'});
stream.write({ text: 'foo and bar and foo'});
stream.end();
'''

Emits the data: '{ text: 'foo and bar and foo', keywords: [foo] }'.

Related projects
----------------
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
