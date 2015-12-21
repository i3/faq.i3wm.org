/* **************************************************** */
// Search query-string manipulation utils
/* **************************************************** */

var QSutils = QSutils || {};  // TODO: unit-test me

QSutils.TAG_SEP = ','; // should match const.TAG_SEP; TODO: maybe prepopulate this in javascript.html ?

QSutils.DEFAULT_QUERY_STRING = 'scope:all/sort:activity-desc/page:1/';

QSutils.get_query_string_selector_value = function (query_string, selector) {
    var params = query_string.split('/');
    for (var i = 0; i < params.length; i++) {
        var param_split = params[i].split(':');
        if (param_split[0] === selector) {
            return param_split[1];
        }
    }
    return undefined;
};

QSutils.patch_query_string = function (query_string, patch, remove) {
    var params = query_string.split('/');
    var patch_split = patch.split(':');

    var new_query_string = '';
    var mapping = {};

    if (!remove) {
        // prepopulate the patched selector if it's not meant to be removed
        mapping[patch_split[0]] = patch_split[1];
    }

    for (var i = 0; i < params.length; i++) {
        var param_split = params[i].split(':');
        if (param_split[0] !== patch_split[0] && param_split[1]) {
            mapping[param_split[0]] = param_split[1];
        }
    }

    var add_selector = function (name) {
        if (name in mapping) {
            new_query_string += name + ':' + mapping[name] + '/';
        }
    };

    /* The order of selectors should match the Django URL */
    add_selector('scope');
    add_selector('sort');
    add_selector('tags');
    add_selector('author');
    add_selector('page');
    add_selector('page-size');
    add_selector('query');

    return new_query_string;
};

QSutils.remove_search_tag = function (query_string, tag) {
    var tag_string = this.get_query_string_selector_value(query_string, 'tags');
    if (!tag_string) {
        return query_string;
    }

    var tags = tag_string.split(this.TAG_SEP);

    var pos = $.inArray(encodeURIComponent(tag), tags);
    if (pos > -1) {
        tags.splice(pos, 1); /* array.splice() works in-place */
    }

    if (tags.length === 0) {
        return this.patch_query_string(query_string, 'tags:', true);
    } else {
        return this.patch_query_string(query_string, 'tags:' + tags.join(this.TAG_SEP));
    }
};

QSutils.add_search_tag = function (query_string, tag) {
    var tag_string = this.get_query_string_selector_value(query_string, 'tags');
    tag = encodeURIComponent(tag);
    if (!tag_string) {
        tag_string = tag;
    } else {
        tag_string = [tag_string, tag].join(this.TAG_SEP);
    }

    return this.patch_query_string(query_string, 'tags:' + tag_string);
};
