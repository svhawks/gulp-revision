# gulp-revision

Customizable asset revisioner for gulp.

**NOTE:** This should not be confused with the very similar [gulp-rev](https://www.npmjs.com/package/gulp-rev) package. gulp-revision offers more flexibility for hashing and transforming but does not work with some of the other tooling around gulp-rev such as [gulp-rev-delete-original](https://github.com/nib-health-funds/gulp-rev-delete-original).

## Install

```
npm install gulp-revision
```


## Usage

```javascript
var gulp = require('gulp');
var revision = require('gulp-revision');

var path = require('path');
var crypto = require('crypto');

var customHasher = function (file) {
    return crypto.createHash('md5').update(file.contents).digest('hex');
};

var customTransformer = function (file, hash) {
    var extension = path.extname(file.path);

    return path.basename(file.path, extension) + '-' + hash + extension;
};

var revisionAssets = function revisionAssets () {

    return gulp.src('source/assets/*.*')
        .pipe(revision({
            hasher: customHasher,
            transformer: customTransformer
        })
        .pipe(gulp.dest('destination/assets/'))
        
        .pipe(revision.manifest({
            path: 'assetRevisionsMap.json'
        }))
        .pipe(gulp.dest('destination/assets/'));

};

gulp.task('default', revisionAssets);
```
