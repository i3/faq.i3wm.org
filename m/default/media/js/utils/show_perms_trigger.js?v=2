var ShowPermsTrigger = function () {
    WrappedElement.call(this);
};
inherits(ShowPermsTrigger, WrappedElement);

ShowPermsTrigger.prototype.decorate = function (element) {
    this._element = element;
    var hoverCard = new PermsHoverCard();
    this._hoverCard = hoverCard;
    $('body').append(hoverCard.getElement());

    hoverCard.setTrigger(this);
    hoverCard.setUrl(element.data('url'));

    var onEnter = hoverCard.getOpenHandler();
    var onExit = hoverCard.getCloseHandler();
    element.hover(onEnter, onExit);
    var onClose = hoverCard.getImmediateCloseHandler();
    $('body').click(onClose);
};
