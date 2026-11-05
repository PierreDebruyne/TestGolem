# Vision

For a home energy management system (with solar panels, home battery, electric heating and in the future an electric car), create a forecast of energy use and energy generation for the next 24 hours. In a next phase this will be used as input to drive the home battery charging or discharging, to avoid high grid imports ("capacity tariff" : try to keep avg use per 15 minutes below 2.5kW). 
 
For the forecast model, use these inputs:
historical usage metrics (available in prometheus) based day-of-week and time-of-day (or combined as time-of-week) to represent our typical household use (cooking etc...)
thermal model of the home, by checking the outdoor temperature vs evolution of indoor temperature when heating is off
same as above when heating is on
temperature forecast to identify the heating need for tomorrow
solar/cloud forecast to identify the electricity generation for tomorrow
keep the code / logic simple. Don't overdo it. Prefer a simple version that covers all aspects over a detailed implementation of one task. Iterate afterwards to improve. Make sure all internal state and intermediate predictions (forecast solar, forecast temp, forecast heating active, ...) can be checked and analysed / compared to the reality later. Exporting those intermediate/internals as prometheus metrics is a good option.



Freedom & Judgment
This document is not exhaustive - it sets the vision, not every detail. You are expected to take initiative and make your own design decisions
Fill in whatever a real, usable launcher needs even if it's not listed here (e.g. configuration, recovery from failures, monitoring, installation on a new machine, etc.) - you decide what's needed and how to build it
Libraries and approaches listed below are suggestions: replace them if you find a better fit, as long as you document the reasoning
Plan the work in milestones of your choosing, and record important decisions in the docs
