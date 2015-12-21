var PermsHoverCard = function () {
    WrappedElement.call(this);
    this._isLoaded = false;
};
inherits(PermsHoverCard, WrappedElement);

PermsHoverCard.prototype.setContent = function (data) {
    this._element.html(data.html);
};

PermsHoverCard.prototype.setTrigger = function (trigger) {
    this._trigger = trigger;
};

PermsHoverCard.prototype.setPosition = function () {
    var trigger = this._trigger.getElement();
    var coors = trigger.offset();
    var height = trigger.outerHeight();
    var triangle = this._element.find('.triangle');
    var triangleHeight = triangle.outerHeight();
    this._element.css({
        'top': coors.top + height + triangleHeight,
        'left': coors.left
    });
};

PermsHoverCard.prototype.setUrl = function (url) {
    this._url = url;
};

PermsHoverCard.prototype.startLoading = function (onLoad) {
    var me = this;
    $.ajax({
        type: 'GET',
        dataType: 'json',
        cache: false,
        url: this._url,
        success: function (data) {
            if (data.success) {
                me.setContent(data);
                me.setPosition();
                onLoad();
                me.setIsLoaded();
            } else {
                notify.show(data.message);
            }
        }
    });
};

PermsHoverCard.prototype.isLoaded = function () {
    return this._isLoaded;
};

PermsHoverCard.prototype.setIsLoaded = function () {
    this._isLoaded = true;
};

PermsHoverCard.prototype.getOpenHandler = function () {
    var me = this;
    return function () {
        me.clearCancelOpenTimeout();
        if (me.isLoaded()) {
            me.getElement().show();
        } else {
            var onload = function () {
                me.getElement().show();
            };
            me.startLoading(onload);
        }
    };
};

PermsHoverCard.prototype.setCancelOpenTimoutId = function (timeoutId) {
    this._cancelOpenTimeoutId = timeoutId;
};

PermsHoverCard.prototype.clearCancelOpenTimeout = function () {
    var timeout = this._cancelOpenTimeoutId;
    if (timeout) {
        clearTimeout(timeout);
    }
};

PermsHoverCard.prototype.getCloseHandler = function () {
    var me = this;
    return function () {
        var element = me.getElement();
        //start timeout to close
        var timeout = setTimeout(function () {
            element.hide();
            //element.fadeOut('fast');
        }, 200);
        me.setCancelOpenTimoutId(timeout);
    };
};

PermsHoverCard.prototype.getImmediateCloseHandler = function () {
    var me = this;
    return function () {
        me.getElement().hide();
    };
};

PermsHoverCard.prototype.getKeepHandler = function () {
    var me = this;
    return function () {
        me.clearCancelOpenTimeout();
    };
};

PermsHoverCard.prototype.createDom = function () {
    var element = this.makeElement('div');
    this._element = element;
    element.addClass('hovercard');
    element.hover(
        this.getKeepHandler(),
        this.getCloseHandler()
    );
    this._element.hide();
};
