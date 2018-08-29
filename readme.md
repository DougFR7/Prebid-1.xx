# Webedia Biblioteca Prebid 1.XX

## Header Bidding onEvent()

Tempo de requisição do Bid
```HTML
pbjs.onEvent('bidRequested', function(data) {
    switch(data.bidderCode) {
      case 'rubicon': pb_request.rubicon = Date.now();
                      break;
      case 'appnexus': pb_request.appnexus = Date.now();
                       break;
      case 'criteo': pb_request.criteo = Date.now();
                     break;
    }
});
```
Tempo de resposta do Bid
```HTML
pbjs.onEvent('bidResponse', function(data) {
    switch(data.bidderCode) {
      case 'rubicon': pb_response.rubicon = Date.now();
                      console.log("Rubicon took " + (pb_response.rubicon - pb_request.rubicon) + " ms to bid for " + data.adUnitCode);
                      break;
      case 'appnexus': pb_response.appnexus = Date.now();
                       console.log("Appnexus took " + (pb_response.appnexus - pb_request.appnexus) + " ms to bid for " + data.adUnitCode);
                       break;
      case 'criteo': pb_response.criteo = Date.now();
                     console.log("Criteo took " + (pb_response.criteo - pb_request.criteo) + " ms to bid for " + data.adUnitCode);
                     break;
    }
});
```

Tempo de inicio do lance inicial e do lance final
```HTML
pbjs.onEvent('auctionInit', function(data) {
    pb_auction.init = Date.now();
});

pbjs.onEvent('auctionEnd', function(data) {
    pb_auction.end = Date.now();
    console.log("Auction took " + (pb_auction.end - pb_auction.init) + " ms.");
});
```

Verifica se a requisição teve o Timeout
```HTML
pbjs.onEvent('bidTimeout', function(data) {
    console.log("Prebid Warning: Bid timed out.")
});
```

## Header Bidding setConfig()

```HTML
pbjs.setConfig({
    debug: false, ///Permite debugar o log///
    priceGranularity: "dense", ///Granularidade///
    enableSendAllBids: true, ///Enviar todos os bids em uma única chamada///
    bidderSequence: "random", ///Sequencia de como são enviados os bids///
    bidderTimeout: 4000, ///Tempo máximo para enviar os Bids///
    publisherDomain: document.domain ///Dominio do site implementado///
});
```

## Header Bidding IDs
```HTML
var formatos = [];
var dominio = window.location.href.match(/:\/\/(.[^/]+)/)[1];
if (dominio == 'blog.tudogostoso.com.br' || dominio == 'blog.hm.tudogostoso.com.br') {
  if (detectmob()){
    formatos = [{id:"ad-header",position:"atf",siteId: "xxxxxx", rubiconId:"xxxxxx",appnexusId:"xxxxxx",criteoId:"xxxxxx"}];
  } else {
    formatos = [{id:"ad-header",position:"atf",siteId: "xxxxxx", rubiconId:"xxxxxx",appnexusId:"xxxxxx",criteoId:"xxxxxx"}];
  }
} else {
  if (detectmob()){
    formatos = [{id:"ad-header",position:"atf",siteId: "xxxxxx", rubiconId:"xxxxxx",appnexusId:"xxxxxx",criteoId:"xxxxxx"}];
  } else {
    formatos = [{id:"ad-header",position:"atf",siteId: "xxxxxx", rubiconId:"xxxxxx",appnexusId:"xxxxxx",criteoId:"xxxxxx"}];
  }
}
```

## Função principal para criar os AdUnits

```HTML
function headerBidding () {
  var units = units || [];
  for (i = 0; i < formatos.length; i++) {
    var div = document.getElementById(formatos[i].id);
    if (div) {
      units.push({
        code: formatos[i].id,
        sizes: stringArray(div.getAttribute('data-mad-size')),
        bids: [{
          bidder: "rubicon",
          params: {
            accountId: "xxxxxx",
            siteId: formatos[i].siteId,
            zoneId: formatos[i].rubiconId,
            inventory: {
              adunit: keywords.adunit.replace('/xxxxxx/', ''),
              position: formatos[i].position
            },
            visitor: profileTailTarget
          }
        },{
          bidder: "appnexus",
          params: {
            placementId: formatos[i].appnexusId
          }
        },{
          bidder: "criteo",
          params: {
            zoneId: formatos[i].criteoId
          }
        }]
      });
    }
  }
  return units;
}
```