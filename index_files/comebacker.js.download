(function(){
    'use strict';
    var ComeBacker = function (options) {
        var _this = this;
        
        /**
         * Дефолтные параметры КБ
         * @type {
         *       landingUrl String - URL лендинга
         *       soundUrk String - URL файла с аудиосообщением
         *       text String - Текст самого камбекера
         *       imageSrc String - URL изображения
         *       innerHtml String - Дополнительный html при необходимости кастомизаций
         *       reinitable Boolean - Режим "надоедливый камбекер"
         * }
         */
        this.params = $.extend({
            landingUrl: '',
            soundUrl: '',
            text: 'Do you really want to leave this page? \n *********************** \n Special deal until the end of the day! \n \n *********************** \n \n Instant savings! Free delivery from the store! \n Hurry up! 44 minutes before this offer is over! \n Click cancel and get a HUGE DISCOUNT!', 
            imageSrc: 'http://cdn.leadbit.com/comebacker/default/comebacker_all_en.jpg',
            innerHtml: undefined,
            inited: false,
            reinitable: false
        }, options);
        
        this.params.text = this.params.text.replace(/\\n/g, "\n");

        //Т.к. бекенд не умеет передавать true или false. Пишем маленький костыль.
        if(this.params.reinitable === '') {
            this.params.reinitable = false;
        }
        if(this.params.landing_complete) {
            this.params.landingUrl = this.params.landing_complete;
        }

        this.el = document.createElement('div');
        this.el.setAttribute('style', 'overflow: hidden; height: 1px; width: 1px');
        this.el.setAttribute('id', 'Comebacker');
        this.el.onmouseover = function (e) {
            $('.Comebacker__header').remove();
        };
        
        var params = this.params;
        
        if(this.params.soundUrl.length) {
            this.Sound = new Audio(this.params.soundUrl);
        }

        //Подписываемся на закрытие окна
        window.onbeforeunload = function() {
            console.log(_this.params.reinitable, _this.params.inited);
            //Если КБ запущен и доступна переинициализация
            if (_this.params.reinitable || !_this.params.inited) {
                _this.unload.call(_this);
                return _this.params.text;
            } 
        };

        // Т.к. браузер Opera осознанно не поддерживает событие onbeforeunload
        // Подисываемся не потерю фокуса окном
        if(typeof window.Opera === 'object') {
            if (_this.params.reinitable || !_this.params.inited) {
                _this.unload.call(_this);
                prompt(_this.params.text);
            }
        }

        this.render();
    };

    ComeBacker.prototype = {

        /**
         * Рендер камбекера
         * @return void
         */
        render: function () {
            //Чистим документ
            document.body.style.height = '100%';
            document.getElementsByTagName('head')[0].style.height = '100%';
            
            //Создаем фрейм
            var iframe = document.createElement('iframe');
                iframe.src = this.params.landingUrl;
                iframe.setAttribute('style', 'width:100%; height: 100%; border: 0; display:block;');

            //Шапка 
            var header = document.createElement('div');
                header.className = 'Comebacker__header';
                header.setAttribute('style', 'width:100%; display: block; text-align:center;');

            //Контент HTML
            if(this.params.innerHtml) {
                var content = document.createElement('div');
                    content.innerHTML = this.params.innerHtml;
                    header.appendChild(content);
            }
            //Баннер
            if(this.params.imageSrc &&  this.params.isImage !== "2") {
                var image = new Image();
                    image.src = this.params.imageSrc;
                    header.appendChild(image);
            }
               
            this.el.appendChild(header);
            this.el.appendChild(iframe);
            document.body.appendChild(this.el);
        },
        
        /**
         * Проигрывание трека
         * @return void 
         */
        playSound: function () {
            this.Sound.play();
        },

        /**
         * Событие закрытия окна
         * @return void 
         */
        unload: function () {
            this.params.inited = true;
            
            //Удаляем все кроме КБ
            $('body').children().not('#Comebacker').remove();
            
            //Убираем стили от проклы
            $('head style, head link').remove();  
            
            //Показываем КБ            
            this.el.setAttribute('style','width:100%;height:100%;overflow:hidden;');

            $('body').append('<style>html{height:100%;}body{margin:0px;padding:0px;height:100%!important;background:none;}</style>');
            if(this.params.soundUrl.length && this.params.isSound!=="2") {
                //Проигрываем трек
                this.playSound();
            }
        }
    };

    window.ComeBacker = ComeBacker;
})();  