
;(function ($, window, document, undefined) {
	'use strict';

	var $document, $window;
	$window = $(window);
	$document = $(document);
	//Page Load
	function pageLoad() {
		var $body 		= $('body');
		$window.on('beforeunload', function() {
			$body.addClass('amy-fade-out');
		});
	}
	
	//General
	function amyGeneral() {
		$('<div/>', {
			id: 'amy-loading'
		}).appendTo('#main');

		$('#amy-loading').append('<span></span>');
	}

	//Menu
	function amyMenu() {
		var $body 		= $('body'),
			click_event	= document.ontouchstart ? 'touchstart' : 'click',
			$overlay;

		if ($body.find('#amy-menu-overlay').length) {
			$overlay	= $body.find('#amy-menu-overlay');
		} else {
			$overlay	= $('<div id="amy-menu-overlay"></div>').prependTo($body);
		}

		$overlay.on(click_event, function() {
			$body.removeClass('amy-menu-toggle-open');
		});

		$('#amy-menu-toggle').on(click_event, function(event) {
			event.preventDefault();
			$body.toggleClass('amy-menu-toggle-open');

		});

		if ($(window).width() < amy_script.viewport) {
			$('body').addClass('hasresponsive');
		} else {
			$('body').removeClass('hasresponsive');
		}

	}

	//Tab
	function amyTab() {
		$(document).on('click.bs.tab.data-api', '.bs-tab-nav a', function (e) {
			e.preventDefault();
			$(this).tab('show');
		});
	}

	//Slider
	function amySlider() {
		var as 			= $('.amy-slick');

		if (amy_script.amy_rtl == 1) {
			as.slick({
				rtl: true
			});
		} else {
			as.slick();
		}

	}

	//fancybox
	function amyFancyBox() {
		$('.amy-fancybox').fancybox();
	}

	//Grid Isotope
	function amyGridIsotope() {
		$('.amy-mv-grid .amy-ajax-content').isotope({
			// options
			itemSelector: '.grid-item',
			layoutMode: 'fitRows'
		});
	}

	//Blog Isotope
	function amyBlogIsotope() {
		$('.amy-masonry .page-content').isotope({
			itemSelector: '.entry-item',
			layoutMode: 'masonry'
		});

		$('.amy-blog.amy-grid .row').isotope({
			itemSelector: '.amy-blog.amy-grid .row > div',
		});
	}

	//Movie Filter
	function amyMovieFilter() {
		$('.amy-datepicker').datepicker({
			dateFormat: 'yy-mm-dd',
			onSelect: function(dateText, inst) {
				amyMovieFilterAjax(this);
			}
		});

		$('.select-genre').change(function() {
			amyMovieFilterAjax(this);
		});

		$('.select-cinema').change(function() {
			amyMovieFilterAjax(this);
		});

		$('.amy-mv-sort').change(function() {
			amyMovieFilterAjax(this);
		});
	}

	function amyMovieFilterAjax(el) {
		var $loading 	= $('#amy-loading'),
			$parent		= $(el).parents('.filter-mv'),
			divf		= $parent.siblings('.amy-ajax-content'),
			genreid		= $parent.find('.select-genre').val(),
			cinemaid	= $parent.find('.select-cinema').val(),
			sortby		= $parent.find('.amy-mv-sort').val(),
			release		= $parent.find('.select-date').val(),
			data_send	= $parent.find('.opt-hidden').attr('data-send'),
			string1		= 'genreid=' + genreid + '&cinemaid=' + cinemaid + '&release=' + release + '&sortby=' + sortby,
			string2		= '&data_send=' + data_send,
			string		= string1 + string2;

		$.ajax({
			method: 'POST',
			url: amy_script.ajax_url,
			data: 'action=amy_movie_ajax_filter&' + string,
			dataType: 'json',
			beforeSend: function() {
				$loading.addClass('open');
			},
			success: function(response) {
				$loading.removeClass('open');
				$(divf).empty();
				$(divf).html(response).hide().fadeIn(2000);

				amyBtnShowtime();
			}
		});
	}

	function amyMovieGridDateFilter() {
		$('.amy-date-filter').find('.single-date').each(function() {
			var $this 		= $(this),
				$loading 	= $('#amy-loading'),
				divf		= $this.parents('.amy-date-filter').siblings('.amy-ajax-content'),
				data_send 	= $this.siblings('.option-hidden').find('.opt-hidden').attr('data-send'),
				data_cat	= $this.siblings('.option-hidden').find('.opt-hidden').attr('data-cat'),
				string		= 'release=' + $this.attr('data-value') + '&data_send=' + data_send;

			$this.click(function() {
				$.ajax({
					method: 'POST',
					url: amy_script.ajax_url,
					data: 'action=amy_movie_ajax_filter&' + string,
					dataType: 'json',
					beforeSend: function() {
						$loading.addClass('open');
					},
					success: function(response) {
						$loading.removeClass('open');
						$(divf).empty();
						$(divf).html(response).hide().fadeIn(2000);

						amyBtnShowtime();
					}
				});
			});
		});

		$('.amy-date-filter').find('.amy-calendar').datepicker({
			dateFormat: 'yy-mm-dd',
			onSelect: function(dateText, inst) {
				var $this 		= $(this),
					$loading 	= $('#amy-loading'),
					divf		= $this.parents('.amy-date-filter').siblings('.amy-ajax-content'),
					data_send 	= $this.parents('.single-calendar').siblings('.option-hidden').find('.opt-hidden').attr('data-send'),
					data_cat	= $this.parents('.single-calendar').siblings('.option-hidden').find('.opt-hidden').attr('data-cat'),
					string		= 'release=' + $this.val() + '&data_send=' + data_send;

				$.ajax({
					method: 'POST',
					url: amy_script.ajax_url,
					data: 'action=amy_movie_ajax_filter&' + string,
					dataType: 'json',
					beforeSend: function() {
						$loading.addClass('open');
					},
					success: function(response) {
						$loading.removeClass('open');
						$(divf).empty();
						$(divf).html(response).hide().fadeIn(2000);

						amyBtnShowtime();
					}
				});
			}
		});

		$('.amy-date-filter').find('.change-layout').click(function() {
			var columm,
				$this 		= $(this);

			if ($this.attr('data-column') == 2) {
				columm = 3;
				$this.attr('data-column', 3);
			} else if ($this.attr('data-column') == 3) {
				columm = 2;
				$this.attr('data-column', 2);
			}

			var $loading 	= $('#amy-loading'),
				divf		= $this.parents('.amy-date-filter').siblings('.amy-ajax-content'),
				data_send 	= $this.siblings('.option-hidden').find('.opt-hidden').attr('data-send'),
				data_cat	= $this.siblings('.option-hidden').find('.opt-hidden').attr('data-cat'),
				string		= 'data_send=' + data_send + '&column=' + columm;

			$.ajax({
				method: 'POST',
				url: amy_script.ajax_url,
				data: 'action=amy_movie_ajax_filter&' + string,
				dataType: 'json',
				beforeSend: function() {
					$loading.addClass('open');
				},
				success: function(response) {
					$loading.removeClass('open');
					$(divf).empty();
					$(divf).html(response).hide().fadeIn(2000);

					amyBtnShowtime();
				}
			});
		});
	}

	//Search button
	function amyMovieSearchAction() {
		$('.amy-mv-search .filter-action li').each(function() {
			var $this = $(this),
				value = $this.attr('data-type');

			$this.click(function() {
				$('.amy-mv-search .filter-action li').removeClass('active');
				$this.parents('.filter-action').siblings('.amy_type').val(value);
				$this.addClass('active');
			});
		});
	}

	//Isotope Metro Slider
	function amyMetroSlider() {
		var $slider = $('.amy-isotope');
		$slider.find('.amy-metroslider').isotope({
			itemSelector: '.item',
			layoutMode: 'masonryHorizontal'
		});

		if ($slider.find('.amy-metroslider').length != 0) {
			$slider.smoothDivScroll({
			    touchScrolling: true,
				autoScrollingMode: 'onStart'
			});
		}

	}

	//Rating star
	function amyRatingStar() {
		$('.mv-current-rating').each(function() {
			var point = $(this).attr('data-point');

			$(this).css('width', point);
		});
	}

	//Shortcode Showtime
	function amyAjaxShowtime() {
		$('input[type=radio][name=movie_id]').change(function() {
			amyAjaxShowtimefn($(this), 'movie');
		});

		$('input[type=radio][name=cinema_id]').change(function() {
			amyAjaxShowtimefn($(this), 'cinema');
		});
	}

	//Shortcode showtime ajax
	function amyAjaxShowtimefn(tag, type) {
		var $loading = $('#amy-loading');

		var mvtime		= tag.parents('.amy-mv-showtime'),
			st_type		= mvtime.find('.showtime-type').val(),
			divresult	= mvtime.find('.list-time > div');

		if (type == 'movie') {
			var cinema 		= mvtime.find('input[type=radio][name=cinema_id]:checked'),
				cinema_id 	= cinema.val(),
				movie_id	= tag.val();
		} else if (type == 'cinema') {
			var movie 		= mvtime.find('input[type=radio][name=movie_id]:checked'),
				movie_id 	= movie.val(),
				cinema_id	= tag.val();
		}

		$.ajax({
			method: 'POST',
			url: amy_script.ajax_url,
			data: 'action=amy_movie_ajax_showtime&cinema_id=' + cinema_id + '&movie_id=' + movie_id + '&st_type=' + st_type + '&action_type=shortcode',
			dataType: 'json',
			beforeSend: function() {
				$loading.addClass('open');
			},
			success: function(response) {
				divresult.empty();
				$loading.removeClass('open');
				divresult.html(response);
			}
		});
	}

	//Button showtime
	function amyBtnShowtime() {
		$('.showtime-btn').each(function() {
			var $parent = $(this).parents('.entry-item'),
				$st		= $parent.find('.entry-showtime');

			$(this).click(function() {
				if ($st.hasClass('show')) {
					$st.removeClass('show');
				} else {
					$st.addClass('show');
				}
			});

		});
	}

	//Single Showtime
	function amySingleShowtime() {
		var $loading = $('#amy-loading');

		$('.entry-showtime .select-cinema ul li').each(function() {
			var $this		= $(this),
				cinema_id 	= $(this).attr('data-cinema'),
				movie_id	= $(this).attr('data-movie'),
				showtime	= $('.entry-showtime .showtime');

			$(this).click(function() {
				$('.entry-showtime .select-cinema ul li').removeClass('active');

				$.ajax({
					type: 'POST',
					url: amy_script.ajax_url + '?action=amy_movie_ajax_showtime&cinema_id=' + cinema_id + '&movie_id=' + movie_id,
					dataType: 'json',
					beforeSend: function() {
						$loading.addClass('open');
					},
					success: function(response) {
						showtime.empty();
						$this.addClass('active');
						$loading.removeClass('open');
						showtime.html(response);
					}
				});
			});
		});
	}

	function amyMovieRate() {
		$('.movie-rating-star').each(function() {
			$(this).click(function() {
				var $loading 	= $('#amy-loading'),
					value 		= $(this).attr('data-value'),
					m_id		= $(this).attr('data-post');

				$.ajax({
					method: 'POST',
					url: amy_script.ajax_url,
					data: 'action=amy_movie_ajax_rate&post_id=' + m_id + '&point=' + value,
					dataType: 'json',
					beforeSend: function() {
						$loading.addClass('open');
					},
					success: function(response) {
						$loading.removeClass('open');

						if (response == -1) {
							alert(amy_script.amy_rate_already);
						} else if (response == 1) {
							alert(amy_script.amy_rate_done);
							location.reload();
						}
					}
				});
			});
		});
	}

	function amyMovieOther() {
		//
		var cp 	= $('.cinema-details'),
			cb	= cp.find('.bg-dl'),
			cg	= cp.find('.cinema-gallery'),
			ci	= cp.find('.cinema-info');

		cb.css('height', cg.height() + ci.height() + 70);
		cg.css('margin-top', '-' + (cg.height() + 100) + 'px');

		//
		var av = $('.amy-mv-list .entry-item'),
			ai = av.find('.entry-thumb img').width(),
			ac = av.find('.entry-content');

		ac.css('margin-left', ai + 27);
	}

	$(document).ready(function() {
		pageLoad();
		amyGeneral();
		amyMenu();
		amyTab();
		amySlider();
		amyFancyBox();
		amyRatingStar();
		amyMovieSearchAction();
		//amyBlogIsotope();
		//amyGridIsotope();
		amyBtnShowtime();
		amyMovieFilter();
		amyMovieGridDateFilter();
		amyMetroSlider();
		amyAjaxShowtime();
		amySingleShowtime();
		amyMovieRate();
		amyMovieOther();

		$(window).on('resize', function() {
			amyMenu();
		});
	});

})(jQuery, window, document);