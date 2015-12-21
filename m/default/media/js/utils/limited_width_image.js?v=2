/**
 * makes images never take more spaces then they can take
 * @param {<Array>} breakPoints
 * @param {number} maxWidth
 * an array of array values like (min-width, width-offset)
 * where min-width is screen minimum width
 * width-offset - difference between the actual screen width and
 * max-width of the image.
 * width-offset may be undefined - this way we know that this is
 * the widest breakpoint and we apply the default max-width
 * instead.
 * We use this offset to calculate max-width in order to
 * have the images fit the layout no matter the size of the image
 */
var LimitedWidthImage = function (breakPoints, maxWidth) {
    /**
     * breakPoints must be sorted in decreasing
     * order of min-width
     */
    this._breakPoints = breakPoints;
    /**
     * this is width for the fully stretched
     * window, above the first widest breakpoint
     */
    this._maxWidth = maxWidth;
    WrappedElement.call(this);
};
inherits(LimitedWidthImage, WrappedElement);

LimitedWidthImage.prototype.getImageWidthOffset = function (width) {
    var numBreaks = this._breakPoints.length;
    var offset = this._breakPoints[0][1];
    for (var i = 0; i < numBreaks; i++) {
        var point = this._breakPoints[i];
        var minWidth = point[0];
        if (width >= minWidth) {
            break;
        } else {
            offset = point[1];
        }
    }
    return offset;
};

LimitedWidthImage.prototype.autoResize = function () {
    var windowWidth = $(window).width();
    //1) find the offset for the nearest breakpoint
    var widthOffset = this.getImageWidthOffset(windowWidth);
    var maxWidth = '100%';
    if (widthOffset !== undefined) {
        maxWidth = windowWidth - widthOffset;
    } else {
        maxWidth = this._maxWidth;
    }
    this._element.css('max-width', maxWidth);
    this._element.css('height', 'auto');
};

LimitedWidthImage.prototype.decorate = function (element) {
    this._element = element;
    this.autoResize();
    var me = this;
    $(window).resize(function () { me.autoResize(); });
};
