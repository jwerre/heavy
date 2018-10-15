# Heavy

Measure process load.

[![Build Status](https://secure.travis-ci.org/hapijs/heavy.png)](http://travis-ci.org/hapijs/heavy)

## Example


	import Heavy from 'heavy';

	const heavy_instance = new Heavy({
	  sampleInterval: 1000,
	});
	const min_time_between_log_messages = 1000;
	const event_loop_logging_threshold = 40;

	let logging_interval;

	function logIfAboveThreshold({ heavy, logger }) {
	  if (!heavy || !heavy.load || !logger) {
	    return;
	  }
	  if (heavy.load.eventLoopDelay > event_loop_logging_threshold) {
	    logger.info({
	      threshold: event_loop_logging_threshold,
	      event_loop_delay: heavy.load.eventLoopDelay,
	      heap_used: heavy.load.heapUsed,
	      rss: heavy.load.rss,
	    }, 'Event loop delay above threshold');
	  }
	}

	export default {
	  startLoggingPerformance({ logger }) {
	    heavy_instance.start();

	    if (logging_interval) {
	      clearInterval(logging_interval);
	    }

	    logging_interval = setInterval(() => {
	      logIfAboveThreshold({ heavy: heavy_instance, logger: logger });
	    }, min_time_between_log_messages);
	  },
	};

Lead Maintainer - [Eran Hammer](https://github.com/hueniverse)
