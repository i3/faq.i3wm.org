var ResponseNotifs = function () {
    WrappedElement.call(this);
};
inherits(ResponseNotifs, WrappedElement);

ResponseNotifs.prototype.showMessage = function (text) {
    this._actionStatus.html(text);
    this._actionStatus.parent().fadeIn('fast');
};

ResponseNotifs.prototype.hideMessage = function () {
    this._actionStatus.fadeOut('fast');
};

ResponseNotifs.prototype.uncheckNotifs = function (memoIds) {
    var snippets = this.getSnippets(memoIds);
    snippets.find('input[type="checkbox"]').prop('checked', false);
};

ResponseNotifs.prototype.clearNewNotifs = function (memoIds) {
    var snippets = this.getSnippets(memoIds);
    snippets.removeClass('new highlight');
};

ResponseNotifs.prototype.makeMarkAsSeenHandler = function () {
    var me = this;
    return function () {
        var memoIds = me.getCheckedMemoIds();
        if (memoIds.length === 0) {
            me.showMessage(gettext('Please select at least one item'));
            return;
        }
        $.ajax({
            type: 'POST',
            cache: false,
            dataType: 'json',
            data: JSON.stringify({'memo_ids': memoIds}),
            url: askbot.urls.clearNewNotifications,
            success: function (response_data) {
                if (response_data.success) {
                    me.hideMessage();
                    me.clearNewNotifs(memoIds);
                    me.uncheckNotifs(memoIds);
                }
            }
        });
    };
};

ResponseNotifs.prototype.makeSelectAllHandler = function () {
    return function () {
        $('.messages input[type="checkbox"]').prop('checked', true);
    };
};

ResponseNotifs.prototype.makeSelectNoneHandler = function () {
    return function () {
        $('.messages input[type="checkbox"]').prop('checked', false);
    };
};

ResponseNotifs.prototype.getSnippets = function (memoIds) {
    var snippets = $();
    for (var i = 0; i < memoIds.length; i++) {
        var memoId = memoIds[i];
        snippets = snippets.add('.message[data-message-id="' + memoId + '"]');
    }
    return snippets;
};

ResponseNotifs.prototype.deleteSnippets = function (memoIds) {
    var snippets = this.getSnippets(memoIds);
    snippets.fadeOut();
};

ResponseNotifs.prototype.getCheckedMemoIds = function () {
    var memoIds = [];
    var checkedCb = $('.messages :checked');
    for (var i = 0; i < checkedCb.length; i++) {
        var cb = $(checkedCb[i]);
        var memoId = cb.parent().data('messageId');
        memoIds.push(memoId);
    }
    return memoIds;
};

ResponseNotifs.prototype.makeDeleteHandler = function () {
    var me = this;
    return function () {
        var memoIds = me.getCheckedMemoIds();
        if (memoIds.length === 0) {
            me.showMessage(gettext('Please select at least one item'));
            return;
        }
        $.ajax({
            type: 'POST',
            cache: false,
            dataType: 'json',
            data: JSON.stringify({'memo_ids': memoIds}),
            url: askbot.urls.deleteNotifications,
            success: function (response_data) {
                if (response_data.success) {
                    me.hideMessage();
                    me.deleteSnippets(memoIds);
                }
            }
        });
        return false;
    };
};

ResponseNotifs.prototype.decorate = function (element) {
    this._element = element;

    this._actionStatus = $('.action-status span');

    var btn = element.find('.js-mark-as-seen');
    setupButtonEventHandlers(btn, this.makeMarkAsSeenHandler());

    btn = element.find('.js-select-all');
    setupButtonEventHandlers(btn, this.makeSelectAllHandler());

    btn = element.find('.js-select-none');
    setupButtonEventHandlers(btn, this.makeSelectNoneHandler());

    btn = element.find('.js-delete');
    setupButtonEventHandlers(btn, this.makeDeleteHandler());
};
