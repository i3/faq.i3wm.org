var Paginator = function () {
    WrappedElement.call(this);
};
inherits(Paginator, WrappedElement);

/**
 * A mandotory method.
 * this method needs to be implemented by the subclass
 * @interface
 * @param data is json dict returted by the server
 */
Paginator.prototype.renderPage = function (data) {
    throw 'implement me in the subclass';
};

/**
 * A mandatory method.
 * @interface - implement in subclass
 * returns url that can be used to retrieve page data
 */
Paginator.prototype.getPageDataUrl = function (pageNo) {
    throw 'implement me in the subclass';
};

/**
 * Optional method
 * @interface - implement in subclass
 * returns url parameters for the page request
 */
Paginator.prototype.getPageDataUrlParams = function (pageNo) {};

Paginator.prototype.setIsLoading = function (isLoading) {
    this._isLoading = isLoading;
};

Paginator.prototype.startLoadingPageData = function (pageNo) {
    if (this._isLoading) {
        return;
    }
    var me = this;
    var requestParams = {
        type: 'GET',
        dataType: 'json',
        url: this.getPageDataUrl(pageNo),
        cache: false,
        success: function (data) {
            me.renderPage(data);
            me.setCurrentPage(pageNo);
            me.setIsLoading(false);
        },
        failure: function () {
            me.setIsLoading(false);
        }
    };
    var urlParams = this.getPageDataUrlParams(pageNo);
    if (urlParams) {
        requestParams.data = urlParams;
    }
    $.ajax(requestParams);
    me.setIsLoading(true);
    return false;
};

Paginator.prototype.getCurrentPageNo = function () {
    var page = this._element.find('.curr');
    return parseInt(page.attr('data-page'));
};

Paginator.prototype.getIncrementalPageHandler = function (direction) {
    var me = this;
    return function () {
        var pageNo = me.getCurrentPageNo();
        if (direction === 'next') {
            pageNo = pageNo + 1;
        } else {
            pageNo = pageNo - 1;
        }
        me.startLoadingPageData(pageNo);
        return false;
    };
};

Paginator.prototype.getWindowStart = function (pageNo) {
    var totalPages = this._numPages;
    var activePages = this._numActivePages;

    //paginator is "short" w/o prev or next, no need to rerender
    if (totalPages === activePages) {
        return 1;
    }

    //we are in leading range
    if (pageNo < activePages) {
        return 1;
    }

    //we are in trailing range
    var lastWindowStart = totalPages - activePages + 1;
    if (pageNo > lastWindowStart) {
        return lastWindowStart;
    }

    return pageNo - Math.floor(activePages / 2);
};

Paginator.prototype.renderPaginatorWindow = function (windowStart) {
    var anchors = this._paginatorAnchors;
    for (var i = 0; i < anchors.length; i++) {
        var anchor = $(anchors[i]);
        removeButtonEventHandlers(anchor);
        var pageNo = windowStart + i;
        //re-render displayed number
        anchor.html(pageNo);
        //re-render the tooltip text
        var labelTpl = gettext('page %s');
        anchor.attr('title', interpolate(labelTpl, [pageNo]));
        //re-render the "page" data value
        anchor.parent().attr('data-page', pageNo);
        //setup new event handler
        var pageHandler = this.getPageButtonHandler(pageNo);
        setupButtonEventHandlers(anchor, pageHandler);
    }
};

Paginator.prototype.renderPaginatorEdges = function (windowStart, pageNo) {
    //first page button
    var first = this._firstPageNav;
    if (windowStart === 1) {
        first.hide();
    } else {
        first.show();
    }

    //last page button
    var lastWindowStart = this._numPages - this._numActivePages + 1;
    var last = this._lastPageNav;
    if (windowStart === lastWindowStart) {
        last.hide();
    } else {
        last.show();
    }

    //show or hide "prev" and "next" buttons
    if (this._numPages === this._numActivePages) {
        this._prevPageButton.hide();
        this._nextPageButton.hide();
    } else {
        if (pageNo === 1) {
            this._prevPageButton.hide();
        } else {
            this._prevPageButton.show();
        }
        if (pageNo === this._numPages) {
            this._nextPageButton.hide();
        } else {
            this._nextPageButton.show();
        }
    }
};

Paginator.prototype.setCurrentPage = function (pageNo) {

    var currPageNo = this.getCurrentPageNo();
    var currWindow = this.getWindowStart(currPageNo);
    var newWindow = this.getWindowStart(pageNo);
    if (newWindow !== currWindow) {
        this.renderPaginatorWindow(newWindow);
    }

    //select the current page
    var page = this._mainPagesNav.find('[data-page="' + pageNo + '"]');
    if (page.length === 1) {
        var curr = this._element.find('.curr');
        curr.removeClass('curr');
        page.addClass('curr');
    }

    //show or hide ellipses (...) and the last/first page buttons
    //newWindow is starting page of the new paginator window
    this.renderPaginatorEdges(newWindow, pageNo);
};

Paginator.prototype.createButton = function (cls, label) {
    var btn = this.makeElement('span');
    btn.addClass(cls);
    var link = this.makeElement('a');
    link.html(label);
    btn.append(link);
    return btn;
};

Paginator.prototype.getPageButtonHandler = function (pageNo) {
    var me = this;
    return function () {
        if (me.getCurrentPageNo() !== pageNo) {
            me.startLoadingPageData(pageNo);
        }
        return false;
    };
};

Paginator.prototype.decorate = function (element) {
    this._element = element;
    var pages = element.find('.page');
    this._firstPageNav = element.find('.first-page-nav');
    this._lastPageNav = element.find('.last-page-nav');
    this._mainPagesNav = element.find('.main-pages-nav');
    var paginatorButtons = element.find('.paginator');
    this._numPages = paginatorButtons.data('numPages');

    var mainNavButtons = element.find('.main-pages-nav a');
    this._paginatorAnchors =  mainNavButtons;
    this._numActivePages = mainNavButtons.length;

    for (var i = 0; i < pages.length; i++) {
        var page = $(pages[i]);
        var pageNo = page.data('page');
        var link = page.find('a');
        var pageHandler = this.getPageButtonHandler(pageNo);
        setupButtonEventHandlers(link, pageHandler);
    }

    var currPageNo = element.find('.curr').data('page');

    //next page button
    var nextPage = element.find('.next');
    this._nextPageButton = nextPage;
    if (currPageNo === this._numPages) {
        this._nextPageButton.hide();
    }

    setupButtonEventHandlers(
        this._nextPageButton,
        this.getIncrementalPageHandler('next')
    );

    var prevPage = element.find('.prev');
    this._prevPageButton = prevPage;
    if (currPageNo === 1) {
        this._prevPageButton.hide();
    }

    setupButtonEventHandlers(
        this._prevPageButton,
        this.getIncrementalPageHandler('prev')
    );
};
