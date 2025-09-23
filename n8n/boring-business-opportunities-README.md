# Boring Business Opportunities Finder

An n8n workflow that automatically identifies profitable opportunities in traditional, stable business sectors - the "boring" businesses that often provide reliable income streams.

## Overview

This workflow runs daily to research and analyze underserved markets in traditional industries, focusing on essential services and products that have:
- Predictable demand patterns
- Recurring revenue potential  
- Low disruption risk from technology
- Clear paths to profitability

## What It Does

1. **Market Research**: Analyzes current market gaps in traditional industries
2. **Opportunity Analysis**: Identifies opportunities in:
   - 🏢 B2B Services (cleaning, maintenance, logistics)
   - 🛠️ Essential Services (plumbing, electrical, HVAC)
   - 📊 Niche Software (industry-specific tools)
   - 🚛 Supply Chain Solutions (distribution, warehousing)
3. **Detailed Reports**: Generates comprehensive analysis including:
   - Market size and trends
   - Competition analysis
   - Startup requirements
   - Financial projections
   - Success factors
   - Risk assessment

## Setup Instructions

### Prerequisites
- n8n instance running
- AI API credentials (Azure OpenAI or OpenAI)
- Gmail account for report delivery

### Configuration Steps

1. **Import the Workflow**
   - Upload `boring-business-opportunities.json` to your n8n instance

2. **Configure AI Agent**
   - Add your Azure OpenAI or OpenAI API credentials
   - Ensure the model is set correctly (default: gpt-4.1)

3. **Setup Email Delivery**
   - Configure Gmail OAuth2 credentials
   - Update the recipient email address in the "Send Opportunity Report" node
   - Default is set to `your-email@example.com` - **make sure to change this**

4. **Customize Schedule**
   - Default: Runs daily
   - Modify the "Daily Trigger" node for different frequency

5. **Geographic Focus**
   - Default focuses on: United States, Canada, United Kingdom
   - Modify in "Set Research Parameters" node if needed

## Customization Options

### Business Sectors
Edit the AI Agent prompt to focus on specific industries:
- Add or remove industry categories
- Adjust criteria for opportunity identification
- Modify geographic scope

### Report Format
Customize the "Format Opportunity Report" node to:
- Change email styling
- Modify report structure
- Add custom branding

### Analysis Depth
Adjust the AI prompt to:
- Focus on specific market sizes
- Target different investment ranges
- Emphasize particular business models

## Output Format

The workflow generates a comprehensive HTML email report containing:

- **Executive Summary**: Key opportunities identified
- **Detailed Analysis**: For each opportunity:
  - Market overview and size
  - Business model breakdown
  - Competitive landscape
  - Startup requirements
  - Financial projections
  - Success factors
  - Risk assessment
  - Next steps for validation

## Example Opportunities

The workflow typically identifies opportunities like:
- Specialized cleaning services (medical facilities, industrial)
- Niche maintenance services (elevator, pool, HVAC)
- Local logistics solutions (same-day delivery, warehousing)
- Industry-specific software tools
- Essential repair services
- Subscription-based B2B services

## Best Practices

1. **Regular Review**: Check reports weekly to identify trends
2. **Market Validation**: Always validate opportunities locally before investing
3. **Resource Assessment**: Match opportunities to your available resources
4. **Start Small**: Begin with minimal viable approach for testing

## Troubleshooting

### Common Issues
- **No email received**: Check Gmail credentials and recipient address
- **Generic responses**: Ensure AI model has sufficient credits
- **Workflow fails**: Verify all node connections are correct

### Support
- Check n8n logs for detailed error messages
- Ensure all required credentials are properly configured
- Verify API rate limits aren't exceeded

## Based On

This workflow is based on the `ai-agent-newsletter.json` template, adapted specifically for business opportunity research in traditional industries.

## License

Same as repository license (MIT).