# api documentation for  [scissors (v0.1.0)](https://github.com/tcr/scissors#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-scissors.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-scissors) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-scissors.svg)](https://travis-ci.org/npmdoc/node-npmdoc-scissors)
#### PDF manipulation in Node.js! Split, join, crop, read, extract, boil, mash, stick them in a stew.

[![NPM](https://nodei.co/npm/scissors.png?downloads=true)](https://www.npmjs.com/package/scissors)

[![apidoc](https://npmdoc.github.io/node-npmdoc-scissors/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-scissors_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-scissors/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-scissors/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-scissors/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Tim Cameron Ryan"
    },
    "bugs": {
        "url": "https://github.com/tcr/scissors/issues"
    },
    "dependencies": {
        "async": "~0.1.22",
        "bufferjs": "~2.0.0",
        "bufferstream": "~0.5.1",
        "temp": "~0.4.0"
    },
    "description": "PDF manipulation in Node.js! Split, join, crop, read, extract, boil, mash, stick them in a stew. ",
    "devDependencies": {},
    "directories": {},
    "dist": {
        "shasum": "e3a0a452420c459e47a2f9f71d6f711d772d7b8b",
        "tarball": "https://registry.npmjs.org/scissors/-/scissors-0.1.0.tgz"
    },
    "homepage": "https://github.com/tcr/scissors#readme",
    "keywords": [
        "pdf",
        "manipulation",
        "postscript",
        "document",
        "split",
        "join",
        "crop"
    ],
    "license": "MIT",
    "main": "scissors.js",
    "maintainers": [
        {
            "name": "timcameronryan",
            "email": "id@timryan.org"
        }
    ],
    "name": "scissors",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/tcr/scissors.git"
    },
    "version": "0.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module scissors](#apidoc.module.scissors)
1.  [function <span class="apidocSignatureSpan">scissors.</span>join ()](#apidoc.element.scissors.join)



# <a name="apidoc.module.scissors"></a>[module scissors](#apidoc.module.scissors)

#### <a name="apidoc.element.scissors.join"></a>[function <span class="apidocSignatureSpan">scissors.</span>join ()](#apidoc.element.scissors.join)
- description and source-code
```javascript
join = function () {
  var args = Array.prototype.slice.call(arguments);

  var outfile = joinTemp + '/' + (joinindex++) + '.pdf';
  var pdf = new Command(outfile, false);

  async.map(args, function (arg, next) {
    var file = joinTemp + '/' + (joinindex++) + '.pdf';
    arg.pdfStream()
      .pipe(fs.createWriteStream(file))
      .on('close', function () {
        next(null, file);
      });
  }, function (err, files) {
    command = ['pdftk'].concat(files, ['output', outfile]);
    var prog = spawn(command[0], command.slice(1));
    console.error('spawn:', command.join(' '));
    prog.stderr.on('data', function (data) {
      process.stderr.write(command[0].match(/[^\/]*$/)[0] + ': ' + String(data));
    });
    prog.on('exit', function (code) {
      if (code) {
        console.error(command[0], 'exited with failure code:', code);
      }
      // PDF is now ready.
      pdf.onready.deliver();
    });
  });

  return pdf;
}
```
- example usage
```shell
...
   .rotate(90) // 90, 180, 270, 360
   .compress()
   .uncompress()
   .crop(100, 100, 300, 200) // offset in points from left, bottom, right, top

// Join multiple files...
var pdfA = scissors('1.pdf'), pdfB = scissors('2.pdf'), pdfC = scissors('3.pdf')
scissors.join(pdfA.page(1), pdfB, pdfC.pages(5, 10)).deflate().pdfStream()...

// And output data as streams.
pdf.pdfStream().pipe(fs.createWriteStream('out.pdf')); // PDF of compiled output
pdf.pngStream(300).pipe(fs.createWriteStream('out-page1.png')); // PNG of first page at 300 dpi
pdf.textStream().pipe(process.stdout) // Individual text strings

// Extract content as text or images:
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
