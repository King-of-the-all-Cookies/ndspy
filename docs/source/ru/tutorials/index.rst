..
    Copyright 2019 RoadrunnerWMC

    This file is part of ndspy.

    ndspy is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    ndspy is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with ndspy.  If not, see <https://www.gnu.org/licenses/>.

Руководства
===========

В этом разделе собраны руководства, которые помогут освоить ndspy.

Рекомендую начать с :doc:`getting-started`, чтобы убедиться, что вы установили
ndspy и запускаете нужную копию Python. Далее переходите к :doc:`rom-editing`,
он даёт мягкое введение в основные приёмы работы. После этого можете изучать
любые другие руководства, которые покажутся интересными.

Предполагается, что вы немного знакомы с Python; всё же особо сложного синтаксиса
мы не используем [1]_, так что даже если вы не ас, переживать не стоит.

Эти руководства используют *New Super Mario Bros.* в качестве примера, потому
что это популярная игра и с ней у меня наибольший опыт. Однако ndspy должен
работать и с другими играми, использующими поддерживаемые форматы файлов.

.. [1]

    Блоки ``with`` для открытия файлов могут выглядеть как исключение, но их
    не обязательно досконально понимать, чтобы пользоваться ими.

.. toctree::
    :maxdepth: 1
    :caption: Страницы

    getting-started
    rom-editing
    narc-editing

.. todo::

    *   Работа с ARM9: измените значение по адресу 0xADDR на VALUE. По сути,
        ручная замена Magigoomba. (Можно упомянуть, что то же можно делать с MG.)

    *   Работа с оверлеями: то же самое, но для оверлеев.

    *   Экспорт/импорт из SDAT: покажите, как вытащить данные из SDAT и затем
        вернуть их обратно. Включите пример экспорта *SWAV* из *SWAR*.

    *   Импорт музыкального трека из одной игры в другую: нужно устанавливать
        ID банков и т.п. Покажите, как последовательности могут делить банки, и
        объясните, почему нельзя просто добавлять новые банки из ниоткуда.

    *   Работа со звуковым эффектом (очень просто): замените один *SWAV* на
        другой — как обсуждалось в Discord.

    *   Работа со звуковым эффектом (базово): измените мелодию 1-UP (может быть
        как в SMM с 1UP, но она не считается).

    *   Работа со звуковым эффектом (средний уровень): замените что-то на один
        *NSMBW WAV*. Потребуется удалить несколько событий последовательности.
        Покажите сначала, как это сделать вручную, а потом — через
        :py:mod:`ndspy.extras.soundEffect`.

    *   Работа со звуковым эффектом (продвинутая): вставьте SFX из NSMBW, у
        которого две ноты и два разных тона! Хороший случай для регионального
        инструмента.

    *   Сжатие/разжатие в формате *LZ10*.

    *   Редактирование сообщений BMG.
