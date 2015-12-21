var setup_inbox = function () {
    var page = $('.inbox-forum');
    if (page.length) {
        var clearNotifs = $('.js-manage-messages');
        if (clearNotifs.length) {
            var inbox = new ResponseNotifs();
            inbox.decorate(clearNotifs);
        }
    }
};

var setup_badge_details_toggle = function () {
    $('.badge-context-toggle').each(function (idx, elem) {
        var context_list = $(elem).parent().next('ul');
        if (context_list.children().length > 0) {
            $(elem).addClass('active');
            var toggle_display = function () {
                if (context_list.css('display') === 'none') {
                    $('.badge-context-list').hide();
                    context_list.show();
                } else {
                    context_list.hide();
                }
            };
            $(elem).click(toggle_display);
        }
    });
};

(function () {
    var fbtn = $('.js-follow-user');
    if (fbtn.length === 1) {
        var toggle = new TwoStateToggle();
        toggle.setDataValidator(
            'success',
            function(data) { return data.status === 'success'; }
        );
        toggle.setDataValidator(
            'enabled',
            function(data) { return data.following; }
        );
        toggle.setBeforeSubmitHandler(function(data) {
            if (!askbot.data.userIsAuthenticated) {
                var message = gettext(
                    'Please <a href="%(signin_url)s">signin</a> to follow %(username)s'
                );
                var message_data = {
                    signin_url: askbot.urls.user_signin + '?next=' + window.location.href,
                    username: askbot.data.viewUserName
                };
                message = interpolate(message, message_data, true);
                showMessage(toggle.getElement(), message);
                return false;
            }
            return true;
        });
        toggle.decorate(fbtn);
    }
    if (askbot.data.userId !== askbot.data.viewUserId) {
        if (askbot.data.userIsAdminOrMod) {
            var group_editor = new UserGroupsEditor();
            group_editor.decorate($('#user-groups'));
        } else {
            $('#add-group').remove();
        }
    } else {
        $('#add-group').remove();
    }

    var tweeting = $('.auto-tweeting');
    if (tweeting.length) {
        var tweetingControl = new Tweeting();
        tweetingControl.decorate(tweeting);
    }

    var qPager = $('.user-questions-pager');
    if (qPager.length) {
        var qPaginator = new UserQuestionsPaginator();
        qPaginator.decorate(qPager);
    }

    var aPager = $('.user-answers-pager');
    if (aPager.length) {
        var aPaginator = new UserAnswersPaginator();
        aPaginator.decorate(aPager);
    }

})();
