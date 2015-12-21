/**
 * controls that set up automatic tweeting to the user account
 */
var Tweeting = function () {
    WrappedElement.call(this);
};
inherits(Tweeting, WrappedElement);

Tweeting.prototype.getStartHandler = function () {
    var url = this._startUrl;
    return function () {
        window.location.href = url;
    };
};

Tweeting.prototype.getMode = function () {
    return this._modeSelector.val();
};

Tweeting.prototype.getModeSelectorHandler = function () {
    var me = this;
    var url = this._changeModeUrl;
    return function () {
        $.ajax({
            type: 'POST',
            dataType: 'json',
            url: url,
            data: {'mode': me.getMode() },
            cache: false
        });
    };
};

Tweeting.prototype.getAccount = function () {
    return this._accountSelector.val();
};

Tweeting.prototype.getAccountSelectorHandler = function () {
    var selectAccountUrl = this._changeModeUrl;
    var startUrl = this._startUrl;
    var me = this;
    return function () {
        var account = me.getAccount();
        if (account === 'existing-handle') {
            $.ajax({
                type: 'POST',
                dataType: 'json',
                url: selectAccountUrl,
                data: {'mode': 'share-my-posts' },
                cache: false
            });
        } else if (account === 'new-handle') {
            window.location.href = startUrl;
        }
    };
};

Tweeting.prototype.decorate = function (element) {
    this._element = element;
    this._changeModeUrl = element.data('changeModeUrl');
    this._startUrl = element.data('startUrl');
    if (element.hasClass('disabled')) {
        this._startButton = element.find('.start-tweeting');
        setupButtonEventHandlers(this._startButton, this.getStartHandler());
    } else if (element.hasClass('inactive')) {
        //decorate choose account selector
        this._accountSelector = element.find('select');
        this._accountSelector.change(this.getAccountSelectorHandler());
    } else if (element.hasClass('enabled')) {
        //decorate choose mode selector
        this._modeSelector = element.find('select');
        this._modeSelector.change(this.getModeSelectorHandler());
    }
};
