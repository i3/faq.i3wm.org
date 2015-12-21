/** wrapper around jQuery object
 * @constructor
 * the top level "class" for other elements
 * I.e. all other things must inherit this class.
 * For an example of the inheritance pattern,
 * please see the "TippedInput" below.
 */
var WrappedElement = function () {
    this._element = null;
    this._in_document = false;
    this._idSeed = null;
};
/* note that we do not call inherits() here
 * See TippedInput as an example of a subclass
 */

/**
 * returns a unique integer for any instance of WrappedElement
 * which can be used to construct a unique id for use in the DOM
 * @return {string}
 */
WrappedElement.prototype.getIdSeed = function () {
    var seed = this._idSeed || parseInt(getNewUniqueInt());
    this._idSeed = seed;
    return seed;
};

/**
 * returns unique ide based on the prefix and the id seed
 * @param {string} prefix
 * @return {string}
 */
WrappedElement.prototype.makeId = function (prefix) {
    return askbotMakeId(prefix, this.getIdSeed());
};

/**
 * notice that we use ObjCls.prototype.someMethod = function ()
 * notation - as we use Javascript's prototypal inheritance
 * explicitly. The point of this is to be able to eventually
 * use the Closure Compiler
 */
WrappedElement.prototype.setElement = function (element) {
    this._element = element;
};

/**
 * this function must be overridden for any object
 * what will use "DOM generation" pattern
 *
 * Inside this function two things can happen:
 * 1) dom structure creation
 * 2) event handlers attached to the dom structure
 */
WrappedElement.prototype.createDom = function () {
    /* inside at the very least you must assign
     * a jQuery object to a parameter called _element
     */
    this._element = $('<div></div>');
};

/**
 * @param {object} element, a jQuery object wrapping a single
 * DOM element.
 *
 * This function must be overridden in the subclasses
 * that are used in the "decoration" pattern
 */
WrappedElement.prototype.decorate = function (element) {
    this._element = element;
};

/**
 * This method should not be overridden
 * Normally you call this method to generate the dom
 * structure, if applicable, or just obtain the
 * jQuery object encapsulating the dom.
 *
 * @return {object} jQuery
 */
WrappedElement.prototype.getElement = function () {
    if (this._element === null) {
        this.createDom();
    }
    return this._element;
};

WrappedElement.prototype.hasElement = function () {
    return (this._element !== undefined);
};
WrappedElement.prototype.inDocument = function () {
    return (this._element && this._element.is(':hidden') === false);
};
WrappedElement.prototype.enterDocument = function () {
    this._in_document = true;
    return this._in_document;
};
WrappedElement.prototype.hasElement = function () {
    return (this._element !== null);
};
/**
 * A utility method, returning a new jQuery object for
 * some HTML tag
 *
 * Example:
 * var ageInput = this.makeElement('input');
 */
WrappedElement.prototype.makeElement = function (html_tag) {
    //makes jQuery element with tags
    return $('<' + html_tag + '></' + html_tag + '>');
};
/**
 * Removes object's DOM element from the DOM tree
 * should be overridden to remove the event handlers
 * and properly destroy the dom structure
 * as well as any other included sub-elements
 */
WrappedElement.prototype.dispose = function () {
    if (this._element) {
        this._element.remove();
    }
    this._in_document = false;
};
