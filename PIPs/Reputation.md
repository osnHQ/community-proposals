# Open Sources Network Reputation System
This system calculates a developer's reputation score based on their activity on GitHub and contributions within the OSN community, with a focus on code committed to specific projects.  
  
## Metrics:  
 - GitHub Activity (for whitelisted/OSN HQ projects only):  
   - Commits (C)  
   - Lines of Code Committed (L)  
   - Merged Pull Requests (PR)  
   - OSN HQ Points (OHP): Points earned through participation in OSN-specific activities (e.g., forum discussions, project reviews)  
 - Weights:  
   - w_C: Weight for Commits (adjustable)  
   - w_L: Weight for Lines of Code (adjustable)  
   - w_PR: Weight for Merged Pull Requests (adjustable)  
   - w_OHP: Weight for OSN HQ Points (adjustable)  
 - Formula:
   - Reputation Score =  
    ( w_C * C * is_OSN_Project(C) ) + 
    ( w_L * L / 10 * is_OSN_Project(L) ) + 
    ( w_PR * PR * is_OSN_Project(PR) ) + 
    w_OHP
  
## Explanation:  
- Each metric is multiplied by its corresponding weight and a function is_OSN_Project(X).  
- The is_OSN_Project function checks if the activity (commit, lines of code, pull request) is associated with a whitelisted project or an OSN HQ project. It returns 1 if it is, and 0 otherwise. This ensures points are only awarded for relevant contributions.  
- Lines of code are divided by 10 before applying the weight to give less emphasis to sheer quantity compared to commits and merged pull requests.  
- OSN HQ Points weight remains independent and is not gated by project association.  
- Weights can be adjusted based on OSN's priorities. For instance, a higher weight for w_L could emphasize code quality over quantity.  
  
## Data Acquisition: 
- GitHub Activity: Utilize the GitHub API to access a developer's commit history, lines of code, and merged pull requests. Filter data based on project association with the is_OSN_Project function.  
- OSN HQ Points: Integrate with OSN's internal system to track a developer's points earned through community participation.  
  
## Cron Job Implementation:  
- Set up a cron job to run periodically (e.g., daily) to:
  - Fetch the latest GitHub data for developers with profiles mentioned in the community.
  - Retrieve current OSN HQ points for those developers.
  - Filter GitHub data for whitelisted/OSN HQ projects using the is_OSN_Project function.
  - Calculate the reputation score using the formula above.
  - Update the developer's profile on OSN with the new reputation score.  

## Additional Considerations for future:
- **Time Decay**: Gradually decrease the weight of older activity to reflect the importance of recent contributions. This can be implemented by adding a decay factor to the formula based on the time elapsed since the activity occurred.  
- **Activity Diversity**: Consider including additional metrics for a more holistic view of contributions, such as code reviews conducted, helpful forum comments, or participation in workshops/events.  
- **Data Normalization**: Normalize the values of each metric (e.g., scaling them to a range of 0-1) before applying weights to ensure a balanced contribution from each factor.    

## Benefits:
- **Motivates developers**: A transparent reputation system encourages participation on GitHub for whitelisted/OSN HQ projects and within the OSN community.
- **Identifies valuable contributors**: Helps recognize developers who are actively contributing code and ideas to relevant projects.  
- **Improves project selection**: Can be used to assess the qualifications of developers interested in contributing to specific OSN projects.  
- **Strengthens community**: Creates a system where valuable contributions to core projects are acknowledged and rewarded.


